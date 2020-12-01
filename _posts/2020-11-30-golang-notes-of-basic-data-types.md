---
layout: post
title: "Go Notes of Basic Data Types (5)"
date: 2020-11-30 00:01:00 +0800
description: "Go Notes of Basic Data Types" # (optional)
img: cover-image-of-golang-notes-series.png # Add image post (optional)
fig-caption: "Go Notes of Basic Data Types" # Add figcaption (optional)
tags: ['Programming', 'Golang']
categories: ['Programming', 'Golang']
---

## Basic Data Types

### Overview

Golang is statically typed programming language meaning that each variable has a type. Go has several built in types that we will look into in this article. Data types in Go can be categorized into the following structure:

- Basic Types
    
    - Integers
        
        - Signed

            - int

            - int8

            - int16 

            - int32 

            - int64
        
        - Unsigned

            - uint

            - uint8

            - uint16

            - uint32

            - uint64

            - uintptr
    
    - Floats

        - float32

        - float64
    
    - Complex Numbers

        - complex64

        - complex128
    
    - Byte
    
    - Rune
    
    - String
    
    - Boolean

- Composite Types

    - Collection/Aggregation or Non-Reference Types

        -Arrays

        -Structs

    - Reference Types

        - Slices

        - Maps

        - Channels

        - Pointers

        - Function/Methods

    - Interface

        - Special case of empty Interface

### Non-Reference Types

- When you assign an array to another variable, it copies the entire array

- When you pass an array as an argument to a function, it makes an entire copy of the array instead of passing just the address

### Channels

Channels provide synchronization and communication between goroutines. You can think of it as a pipe through which goroutines can send values and receive values. The operation <- is used to send or receive, with direction of arrow specifying the direction of flow of data

```go
ch <- val    //Sending a value present in var variable to channel
val := <-cha  //Receive a value from  the channel and assign it to val variable
```

Channel are of two types


- Unbuffered Channel- It doesn't have any capacity to hold and values and thus

    - Send on a channel is block unless there is another goroutine to receive.

    - Receive is block until there is another goroutine on the other side to send.

- Buffered Channel- You can specify the size of buffer here and for them

    - Send on a buffer channel only blocks if the buffer is full

    - Receive is the only block is buffer of the channel is empty

A  channel holds data of a particular type at a time. While creating a channel, the type of data l has to be specified while initializing a new channel. Channel can be created using make. In the below example, we are creating a channel which holds data of type string.

```go
events := make(chan string)  //Unbuffered channel
events2 := make(chan string, 2)  //Buffered channel of length 2
```

Closing a channel

The **close()** function can be used to close a channel. Closing a channel means that no more values can be sent to the channel

Let's see a working code example of both buffered and unbuffered channel

**Buffered Channel Example:**

```go
package main

import "fmt"

func main() {
    //Creating a buffered channel of length 3
    eventsChan := make(chan string, 3)
    eventsChan <- "a"
    eventsChan <- "b"
    eventsChan <- "c"
    //Closing the channel
    close(eventsChan)
    for event := range eventsChan {
        fmt.Println(event)
    }
}

// Output:

a
b
c
```

**UnBuffered Channel Example:**

```go
package main

import "fmt"

func main() {
    eventsChan := make(chan string)
    go sendEvents(eventsChan)
    for event := range eventsChan {
        fmt.Println(event)
    }
}

func sendEvents(eventsChan chan<- string) {
    eventsChan <- "a"
    eventsChan <- "b"
    eventsChan <- "c"
    close(eventsChan)
}

// Output:

a
b
c
```

### IOTA in Golang

Iota is an identifier which is used with constant and which can simplify constant definitions that use auto increment numbers.  The IOTA keyword represent integer constant starting from zero.  So essentially it can be used to create effective constant in Go . They can also be used to create enum in Go.

Auto increment constant without IOTA

```go
const (
    a = 0
    b = 1
    c = 2
)
```

Auto increment constant with IOTA

```go
const (
    a = iota
    b
    c
)
```

So IOTA is

- A counter which starts with zero

- Increases by 1 after each line

- Is only used with constant

IOTA starts with zero and increases by 1 after each line but there are some caveats as well.

```go
package main

import "fmt"

const (
    a = iota
    b
    c
)
func main() {
    fmt.Println(a)
    fmt.Println(b)
    fmt.Println(c)
}
```

**Output:**

```go
0
1
2
```

Iota sets the value of a to zero. Then on each new line and it increments the value by one. Therefore the output is 0 followed by 1 followed by 2

- Iota keyword can be used on each line as well. In that case, also iota will start from zero and increment on each new line. It will be the same as the above case

- iota keyword can be skipped as well. In that case, also iota will start from zero and increment on each new line. It is same as above two cases

- There will be no increment if there is a empty line or a commented line

- **Iota value will reset and again start with zero if the const keyword is used again**

- **iota increment can be skipped using a blank identifier**

- iota expressions – iota allows expressions which can be used to set any value for the constant

- iota can also start from non-zero number- iota expressions can also be used to start iota from any number

### Enum in Golang

IOTA provides an automated way to create a enum in Golang. Let’s see an example.

```go
package main

import "fmt"

type Size uint8

const (
    small Size = iota
    medium
    large
    extraLarge
)

func main() {
    fmt.Println(small)
    fmt.Println(medium)
    fmt.Println(large)
    fmt.Println(extraLarge)
}
```

**Output:**

```go
0
1
2
3
```
