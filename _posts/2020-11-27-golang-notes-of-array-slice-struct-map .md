---
layout: post
title: "Go Notes of Array, Slice, Struct and Map (2)"
date: 2020-11-27 00:01:00 +0800
description: "Go Notes of Array, Slice, Struct and Map" # (optional)
img: cover-image-of-golang-notes-series.png # Add image post (optional)
fig-caption: "Go Notes of Array, Slice, Struct and Map" # Add figcaption (optional)
tags: ['Programming', 'Golang']
categories: ['Programming', 'Golang']
---

## Array

### Defination

An array is a contiguous collection of elements of the same type. It is an ordered sequence of elements stored contiguously in memory

Array are value type in go. So an array variable name is not a pointer to the first element in fact it denotes the entire array. A copy of the array will be created when

- An array variable is assigned to another array variable.

- An array variable is passed as an argument to a function.

### Declaration of an array

Both number of elements and actual elements are optional in the array declaration.

In below example, we see 4 ways of declaring of an array

- Specifying both the length of the array and actual elements. Eg.

```go
[2]int{1, 2}
```

- Only length – In this case all the actual elements are filled up with default value zero of that type. Eg

```go
[2]int{}
```

- Only actual elements – In this case, the length of array will be equal to the number of actual elements. The symbol ‘…’ needs to be used within square brackets like this […] when not specifying the length. The symbol is an instruction to the compiler to calculate the length.

```go
[...]int{2, 3}
```

- Without length and actual elements – an empty array will be created in this case. Similar to above the symbol ‘…’ also needs to be used in this case as well.

```go
[...]int{}
```

Let’s see a code example illustrating above points. Also please keep in mind that the builtin function len() can be used to calculate the length of an array. In below program we are using len() function to calculate the length of the array.

```go
package main

import "fmt"

func main() {
    //Both number of elements and actual elements
    sample1 := [2]int{1, 2}
    fmt.Printf("Sample1: Len: %d, %v\n", len(sample1), sample1)

    //Only actual elements
    sample2 := [...]int{2, 3}
    fmt.Printf("Sample2: Len: %d, %v\n", len(sample2), sample2)

    //Only number of elements
    sample3 := [2]int{}
    fmt.Printf("Sample3: Len: %d, %v\n", len(sample3), sample3)

    //Without both number of elements and actual elements
    sample4 := [...]int{}
    fmt.Printf("Sample4: Len: %d, %v\n", len(sample4), sample4)

    sample5 := [4]int{5, 8}
    fmt.Printf("Sample5: Len: %d, %v\n", len(sample5), sample5)
}
```

**Output**

```go
Sample1: Len: 2, [1 2]
Sample2: Len: 2, [2 3]
Sample3: Len: 2, [0 0]
Sample4: Len: 0, []
Sample5: Len: 4, [5 8 0 0]
```

### Different ways of iterating an array

An array can be iterated using:

- Using for loop
- Using for-range loop
- Let’s see a code example for both

```go
package main

import "fmt"

func main() {
    letters := [3]string{"a", "b", "c"}
    //Using for loop
    fmt.Println("Using for loop")
    len := len(letters)
    for i := 0; i < len; i++ {
        fmt.Println(letters[i])
    }
    //Using for-range operator
    fmt.Println("\nUsing for-range loop")
    for i, letter := range letters {
        fmt.Printf("%d %s\n", i, letter)
    }
}
```

Output

```go
Using for loop
a
b
c

Using for-range loop
0 a
1 b
2 c
```

## Slice

### Defination

A slice points to an underlying array and is internally represented by a slice header.  Unlike array, the size of a slice is flexible and can be changed.

Internally a slice is represented by three things.

- Pointer to the underlying array

- Current length of the underlying array

- Total Capacity which is the maximum capacity to which the underlying array can expand.

The Pointer field in the slice header is a pointer to the underlying array.  Len is the current length of the slice and Cap is the capacity of the slice. Similar to array a slice index starts from zero till length_of_slice-1. So a slice of 3 lengths and 5 capacity will look like below

![a slice of 3 lengths and 5 capacity]({{site.baseurl}}/assets/img/2020-11-28-notes-of-golang-slice-01.jpg)

We also have two library functions provided by go which can be used to know the length and capacity of a slice.

- len() function – for  length of the slice

- cap() function – for capacity of the slice

### Using the make function

make is a builtin function provided by go that can also be used to create a slice. Below is the signature of make function

```go
func make([], length, capacity int) []
```

Capacity is an optional parameter while creating slice using the make function. When capacity is omitted, the capacity of the slice is equal length specified for the slice. When using make function, behind the scenes go allocates an array equal to the capacity. All the elements of the allocated array are initialized with default zero value of the type. Let’s see a program illustrating this point.

### Length vs Capacity

Before moving further, let’s emphasis on understanding the caveats of length and capacity. Let’s create a simple slice with capacity greater than length.

```go
numbers := make([]int, 3, 5)
```

Accessing the slice behind its length will result in a run time error “Index out of range”. It doesn’t matter if the accessed index is within the capacity. So the below line will cause the run time error.

```go
numbers[4] = 5
```

The length of the slice can be increased up to its capacity by re-slicing. So below re-slice will increase the length from 3 to 5.

```go
numbers = numbers[0:5]
```

The length of the slice can also be decreased using re-slicing. So below re-slice will decrease the length from 3 to 2

```go
numbers = numbers[0:2]
```

The advantage of having capacity is that array of size capacity can be pre-allocated during the initialization.  This is a performance boost as if more elements are needed to include in this array then space is already allocated for them.

### Accessing and Modifying Slice Elements

A slice element can be accessed by specifying the index. Slice element can also be allotted a new value using the index. Also, note that any changes in the underlying array will reflect back in the slice as we have also seen above. Let’s see a small example of accessing and modifying

### Appending to a slice

go builtin package provides an append function that can be used to append to a slice at the end. Below is the signature of this function

```go
func append(slice []Type, elems ...Type) []Type

numbers := []int{1,2}
// ATTENTION OF THE 'numbers' used in append
numbers := append(numbers, 4) //Slice will become [1, 2, 3, 4]
```

### Copy a slice

go builtin package provides copy function that can be used to copy a slice. Below is the signature of this function. It takes in two slices dst and src, and copies data from src to dst. It returns the number of elements copied.

```go
func copy(dst, src []Type) int

src := []int{1, 2, 3, 4, 5}
dst := make([]int, 5)

numberOfElementsCopied := copy(dst, src)
```

Basically the number of elements copied is minimum of length of (src, dst). 

### Nil Slice

The default zero value of a slice is nil. The length and capacity both of a nil slice is zero. Though it is possible to append to a nil slice as well. 

## Map

### Defination

Maps are golang builtin datatype similar to the hash table which maps a key to a value. Map is an unordered collection where each key is unique while values can be the same for two or more different keys. The advantages of using a map are that it provides fast retrieval, search, insert, and delete operations.

Maps are referenced data types. When you assign one map to another both refer to the same underlying map. 

```go
map[key_type]value_type
map[string]int
```

Both key_type and value_type can be of different type or same type. For below example the key type is string and value type is int.

### Allowed Key types in a Map

The map key can be any type that is comparable. Some of the comparable types as defined by go specification are

- boolean

- numeric

- string

- pointer

- channel

- interface types

- structs – if all it’s field type is comparable

- array – if the type of value of array element is comparable

Some of the types which are not comparable as per go specification and which cannot be used as a key in a map are.

- Slice

- Map

- Function

### Map Operations

The below operations are applicable for map

- Add a key-value pair

- Update a key

- Get the value corresponding to a key

- Delete a key-value pair

- Check if a key exists

### Check if a key exists

Below is the format to check if a key exist in the map

```go
val, ok := mapName[key]
```

### Zero Value

zero value of a map is nil. This is also proved when we declare a map using the var keyword. See below program.

### Maps are referenced data types

Map are reference data types. So on assigning one map to a new variable, then both variable refers to the same map. Any change in one of the map would reflect in other and vice versa.

### Maps are not safe for concurrent use

go maps are not safe for concurrent use.

Buggy code: Below is a buggy code. It might result in crash if concurrent read and write of map happens.

```go
package main

var (
   allData = make(map[string]string)
)

func get(key string) string {
    return allData[key]
}

func set(key string, value string) {
    allData[key] = value
}

func main() {
    go set("a", "Some Data 1")
    go set("b", "Some Data 2")
    go get("a")
    go get("b")
    go get("a")
}
```

**Possible Output:**

```go
fatal error: concurrent map read and map write
```

**Correct Code:**

```go
package main

import (
    "fmt"
    "sync"
)

var (
    allData = make(map[string]string)
    rwm     sync.RWMutex
)

func get(key string) string {
    rwm.RLock()
    defer rwm.RUnlock()
    return allData[key]

}

func set(key string, value string) {
    rwm.Lock()
    defer rwm.Unlock()
    allData[key] = value

}

func main() {
    set("a", "Some Data")
    result := get("a")
    fmt.Println(result)
}
```

## Struct

### Defination

GO struct is named collection of data fields which can be of different types. Struct acts as a container that has different heterogeneous data types which together represents an entity.

A struct in golang can be compared to a class in Object Oriented Languages

### Declaring a struct type

```go
type struct_name struct {
    field_name1 field_type1
    field_name2 field_type2
    ...
}
```

### Pointer to a struct

There are two ways of creating a pointer to the struct

- Using the & operator

- Using the new keyword

The & operator can be used to get the pointer to a struct variable.

```go
emp := employee{name: "Sam", age: 31, salary: 2000}
empP := &emp

OR

empP := &employee{name: "Sam", age: 31, salary: 2000}
```

Using the  new() keyword will:

- Create the struct

- Initialize all the field to the zero default value of their type

- Return the pointer to the newly created struct

```go
empP := new(employee)

// Pointer address can be print using the %p format modifier
fmt.Printf("Emp Pointer: %p\n", empP)

// Deference operator ‘*’ can be used to print the value at the pointer.
fmt.Printf("Emp Value: %+v\n", *empP)

// Emp Value: {name: age:0 salary:0}

// When not using the dereference pointer but using the format identifier  %+v, then ampersand will be appended before the struct indicating that is a pointer.
fmt.Printf("Emp Value: %+v\n", empP)

// Emp Value: &{name: age:0 salary:0}
```

### Print a Struct Variable

There are two ways to print all struct variables including all its key and values.

- Using the fmt package

- Printing the struct in JSON form using the json/encoding package. This also allows pretty print of a struct as well.

fmt.Printf() function can be used to print a struct.  Different format identifiers can be used to print a struct in different ways.

```go
emp := employee{name: "Sam", age: 31, salary: 2000}

// %v – It will print only values. Field name will not be printed. This is the default way of printing a struct
fmt.Printf("%v", emp)  -  {Sam 31 2000}

// %+v – It will print both field and value.
fmt.Printf("%+v", emp) - {name:Sam age:31 salary:2000}
```

Second method is to print the struct in the JSON format. Marshal and MarshalIndent function of encoding/json package can be used to print a struct in JSON format.

- Marshal – Below is the signature of the Marshal function. This function returns the JSON encoding of v by traversing the value recursively

```go
Marshal(v interface{}) ([]byte, error)
```

- MarshalIndent– Below is the signature of the MarshalIndent function. It is similar to Marshal function but applies Indent to format the output. So it can be used to pretty print a struct

```go
MarshalIndent(v interface{}, prefix, indent string) ([]byte, error)
```

It is to be noted that both Marshal and MarshalIndent function can only access the exported fields of a struct, which means that only the capitalized fields can be accessed and encoded in JSON form.

```go
package main

import (
    "encoding/json"
    "fmt"
    "log"
)

type employee struct {
    Name   string
    Age    int
    salary int
}

func main() {
    emp := employee{Name: "Sam", Age: 31, salary: 2000}
    //Marshal
    empJSON, err := json.Marshal(emp)
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Printf("Marshal funnction output %s\n", string(empJSON))

    //MarshalIndent
    empJSON, err = json.MarshalIndent(emp, "", "  ")
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Printf("MarshalIndent funnction output %s\n", string(empJSON))
}
```

### Struct Field Meta or Tags

A struct in go also allows adding metadata to its fields. These meta fields can be used to encode decode into different forms, doing some forms of validations on struct fields, etc. So basically any meta information can be stored with fields of a struct and can be used by any package or library for different purposes.

```go
type strutName struct{
   fieldName type `key:value key2:value2`
}

type employee struct {
    Name   string `json:"n"`
    Age    int    `json:"a"`
    Salary int    `json:"s"`
}
```

Let’s see full program

```go
package main

import (
    "encoding/json"
    "fmt"
    "log"
)

type employee struct {
    Name   string `json:"n"`
    Age    int    `json:"a"`
    Salary int    `json:"s"`
}

func main() {
    emp := employee{Name: "Sam", Age: 31, Salary: 2000}
    //Converting to jsonn
    empJSON, err := json.MarshalIndent(emp, "", "  ")
    if err != nil {
        log.Fatalf(err.Error())
    }
    fmt.Println(string(empJSON))
}
```

### Anonymous Fields in a Struct

A struct can have anonymous fields as well, meaning a field having no name. The type will become the field name.

```go
package main

import "fmt"

type employee struct {
    string
    age    int
    salary int
}

func main() {
    emp := employee{string: "Sam", age: 31, salary: 2000}
    //Accessing a struct field
    n := emp.string
    fmt.Printf("Current name is: %s\n", n)
    //Assigning a new value
    emp.string = "John"
    fmt.Printf("New name is: %s\n", emp.string)
}
```

### Struct Equality

The first thing to know before considering struct equality is weather if all struct fields types are comparable or not

Some of the comparable types as defined by go specification are

- boolean

- numeric

- string,

- pointer

- channel

- interface types

- structs – if all it’s field type is comparable

- array – if the type of value of array element is comparable

Some of the types which are not comparable as per go specification and which cannot be used as a key in a map are.

- Slice

- Map

- Function

So two struct will be equal if first all their field types are comparable and all the corresponding field values are equal.

### Struct are value types

A struct is value type in go. So a struct variable name is not a pointer to the struct in fact it denotes the entire struct. A new copy of the struct will be created when

- A struct variable is assigned to another struct variable.

- A struct variable is passed as an argument to a function.

```go
package main

import "fmt"

type employee struct {
    name   string
    age    int
    salary int
}

func main() {
    emp1 := employee{name: "Sam", age: 31, salary: 2000}
    fmt.Printf("Emp1 Before: %v\n", emp1)

    emp2 := emp1

    emp2.name = "John"
    fmt.Printf("Emp1 After assignment: %v\n", emp1)
    fmt.Printf("Emp2: %v\n", emp2)

    test(emp1)
    fmt.Printf("Emp1 After Test Function Call: %v\n", emp1)
}

func test(emp employee) {
    emp.name = "Mike"
    fmt.Printf("Emp in Test function: %v\n", emp)
}
```
