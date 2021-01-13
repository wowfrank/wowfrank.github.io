---
layout: post
title: "The Guide to Work With Tags in Golang"
date: 2021-01-13 00:16:00 +0800
description: "The Guide to Work With Tags in Golang" # (optional)
img: 2021-01-13-tags-in-golang-cover.jpeg # Add image post (optional)
fig-caption: "The Guide to Work With Tags in Golang" # Add figcaption (optional)
tags: ['Programming', 'Golang']
categories: ['Programming', 'Golang']
---

> Declaration of struct fields can be enriched by string literal placed afterwards — tag. Tags add meta information used either by current package or external ones. Let’s first recall how struct declarations look like, then we’ll deep dive into tags themselves and we’ll wrap up with couple of use cases.

## Struct type

Struct is a sequence of fields. Each field consists of optional name and required type

```golang
package main
import "fmt"
type T1 struct {
    f1 string
}
type T2 struct {
    T1
    f2     int64
    f3, f4 float64
}
func main() {
    t := T2{T1{"foo"}, 1, 2, 3}
    fmt.Println(t.f1)    // foo
    fmt.Println(t.T1.f1) // foo
    fmt.Println(t.f2)    // 1
}
```

> Field T1 is called an embedded field since it’s declared with type but no name.

Field declaration can specify more than one identifier like f3 and f4 from 3rd field declaration in T2.

Language specification states that each field declaration is followed by semicolon but as we’ve seen above it can be omitted. Semicolon might be useful if there is a need to put multiple fields declarations into the same line

```golang
package main
import "fmt"
type T struct {
    f1 int64; f2 float64
}
func main() {
    t := T{1, 2}
    fmt.Println(t.f1, t.f2)  // 1 2
}
```

## Tag

A field declaration may be followed by an optional string literal (tag) which becomes an attribute of all the fields in the corresponding field declaration (single field declaration can specify multiple identifiers). Let’s see it in action

```golang
type T struct {
    f1     string "f one"
    f2     string
    f3     string `f three`
    f4, f5 int64  `f four and five`
}
```

> Either raw string literals or interpreted string literals can be used but conventional format described below requires raw string literals. Differences between raw and interpreted string literals are described in spec.

If field declarations contains more than one identifier then tag is attached to all fields from field declaration (like fields f4 and f5 above).

### Reflection

Tags are accessible through reflect package which allows run-time reflection

```golang
package main
import (
    "fmt"
    "reflect"
)
type T struct {
    f1     string "f one"
    f2     string
    f3     string `f three`
    f4, f5 int64  `f four and five`
}
func main() {
    t := reflect.TypeOf(T{})
    f1, _ := t.FieldByName("f1")
    fmt.Println(f1.Tag) // f one
    f4, _ := t.FieldByName("f4")
    fmt.Println(f4.Tag) // f four and five
    f5, _ := t.FieldByName("f5")
    fmt.Println(f5.Tag) // f four and five
}
```

Setting up empty tag has the same effect as not using tag at all

```golang
type T struct {
    f1 string ``
    f2 string
}
func main() {
    t := reflect.TypeOf(T{})
    f1, _ := t.FieldByName("f1")
    fmt.Printf("%q\n", f1.Tag) // ""
    f2, _ := t.FieldByName("f2")
    fmt.Printf("%q\n", f2.Tag) // ""
}
```

### Conventional format

![Tags in Golang - Conventional Format]({{site.baseurl}}/assets/img/2021-01-13/tags-in-golang-conventional-format.jpeg)

Introduced in commit “reflect: support for struct tag use by multiple packages” allows to set meta information per package. This provides simple namespacing. Tags are formatted as a concatenation of key:"value" pairs. Key might be name of the package like json. Pairs can be optionally separated by spaces — key1:"value1" key2:"value2" key3:"value3". If conventional format is used then we can use two methods of struct tag (StructTag) — Get or Lookup. They allow to return value associated with desired key inside tag.

Lookup function returns two values — value associated with key (or blank if not set) and bool indicating if key has been found at all

```golang
type T struct {
    f string `one:"1" two:"2"blank:""`
}
func main() {
    t := reflect.TypeOf(T{})
    f, _ := t.FieldByName("f")
    fmt.Println(f.Tag) // one:"1" two:"2"blank:""
    v, ok := f.Tag.Lookup("one")
    fmt.Printf("%s, %t\n", v, ok) // 1, true
    v, ok = f.Tag.Lookup("blank")
    fmt.Printf("%s, %t\n", v, ok) // , true
    v, ok = f.Tag.Lookup("five")
    fmt.Printf("%s, %t\n", v, ok) // , false
}
```

Get method is simply wrapper of Lookup which discards boolean flag

```golang
func (tag StructTag) Get(key string) string {
    v, _ := tag.Lookup(key)
    return v
}
```

> Return value of Get or Lookup is unspecified if tag doesn’t have conventional format.

Even if tag is any string literal (interpreted or raw) then Lookup and Get methods will find value for key only if value is enclosed between double quotes

```golang
type T struct {
    f string "one:`1`"
}
func main() {
    t := reflect.TypeOf(T{})
    f, _ := t.FieldByName("f")
    fmt.Println(f.Tag) // one:`1`
    v, ok := f.Tag.Lookup("one")
    fmt.Printf("%s, %t\n", v, ok) // , false
}
```

It’s possible to use escaped double quotes within interpreted strings literals

```golang
type T struct {
    f string "one:\"1\""
}
func main() {
    t := reflect.TypeOf(T{})
    f, _ := t.FieldByName("f")
    fmt.Println(f.Tag) // one:"1"
    v, ok := f.Tag.Lookup("one")
    fmt.Printf("%s, %t\n", v, ok) // 1, true
}
```

but it’s much less readable.

## Conversion

Converting struct type value into other type requires that underlaying types are identical but tags are ignored

```golang
type T1 struct {
    f int `json:"foo"`
}
    f int `json:"bar"`
}
t1 := T1{10}
var t2 T2
t2 = T2(t1)
fmt.Println(t2) // {10}
```

> This behaviour has been introduced in Go 1.8 (proposal). In Go 1.7 and older above code could would throw a compile-time error.

## Use cases

### (Un)marshaling

Probably the most common use of tags in Go is marshalling. Let’s see how it’s used by function Marshal from json package

```golang
import (
    "encoding/json"
    "fmt"
)
func main() {
    type T struct {
       F1 int `json:"f_1"`
       F2 int `json:"f_2,omitempty"`
       F3 int `json:"f_3,omitempty"`
       F4 int `json:"-"`
    }
    t := T{1, 0, 2, 3}
    b, err := json.Marshal(t)
    if err != nil {
        panic(err)
    }
    fmt.Printf("%s\n", b) // {"f_1":1,"f_3":2}
}
```

Package xml also takes advantage of tags — https://golang.org/pkg/encoding/xml/#MarshalIndent.

## ORM

Object-relation mapping tools like GORM use tags extensively — example.

### Digesting forms data

[https://godoc.org/github.com/gorilla/schema](https://godoc.org/github.com/gorilla/schema)

### Other

There’re more potential uses cases of tags like configuration management, default values for structs, validation, command-line arguments description etc. (list of well-known struct tags).

### go vet

Go compiler doesn’t enforce conventional format of struct tags but go vet does that so it’s worth to use it e.g. as a part of CI pipeline.

```golang
package main
type T struct {
    f string "one two three"
}
func main() {}
> go vet tags.go
tags.go:4: struct field tag `one two three` not compatible with reflect.StructTag.Get: bad syntax for struct tag pairs
```

##### 文章转载自**[Tags in Golang](https://medium.com/golangspec/tags-in-golang-3e5db0b8ef3e){:target="_blank"}**！