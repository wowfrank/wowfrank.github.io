---
layout: post
title: "Introduction of C++ Arrays-Strings-Pointers-and-References (4)"
date: 2021-02-28 00:04:00 +0800
description: "Introduction of C++ Arrays-Strings-Pointers-and-References (4)" # (optional)
img: 2021-02-28-cover-image-of-cpp-notes.jpg # Add image post (optional)
fig-caption: "Introduction of C++ Arrays-Strings-Pointers-and-References (4)" # Add figcaption (optional)
tags: ['Programming', 'C++']
categories: ['Programming', 'C++']
---

## Passing arrays to functions

Although passing an array to a function at first glance looks just like passing a normal variable, underneath the hood, C++ treats arrays differently.

When a normal variable is passed by value, C++ copies the value of the argument into the function parameter. Because the parameter is a copy, changing the value of the parameter does not change the value of the original argument.

However, because copying large arrays can be very expensive, C++ does not copy an array when an array is passed into a function. Instead, the actual array is passed. This has the side effect of allowing functions to directly change the value of array elements!

```cpp
#include <iostream>
 
void passValue(int value) // value is a copy of the argument
{
    value = 99; // so changing it here won't change the value of the argument
}
 
void passArray(int prime[5]) // prime is the actual array
{
    prime[0] = 11; // so changing it here will change the original argument!
    prime[1] = 7;
    prime[2] = 5;
    prime[3] = 3;
    prime[4] = 2;
}
 
int main()
{
    int value{ 1 };
    std::cout << "before passValue: " << value << '\n';
    passValue(value);
    std::cout << "after passValue: " << value << '\n';
 
    int prime[5]{ 2, 3, 5, 7, 11 };
    std::cout << "before passArray: " << prime[0] << " " << prime[1] << " " << prime[2] << " " << prime[3] << " " << prime[4] << '\n';
    passArray(prime);
    std::cout << "after passArray: " << prime[0] << " " << prime[1] << " " << prime[2] << " " << prime[3] << " " << prime[4] << '\n';
 
    return 0;
}

// output
// before passValue: 1
// after passValue: 1
// before passArray: 2 3 5 7 11
// after passArray: 11 7 5 3 2
```

As a side note, if you want to ensure a function does not modify the array elements passed into it, you can make the array const:

```cpp
// even though prime is the actual array, within this function it should be treated as a constant
void passArray(const int prime[5])
{
    // so each of these lines will cause a compile error!
    prime[0] = 11;
    prime[1] = 7;
    prime[2] = 5;
    prime[3] = 3;
    prime[4] = 2;
}
```

### What good are pointers?

At this point, pointers may seem a little silly, academic, or obtuse. Why use a pointer if we can just use the original variable?

It turns out that pointers are useful in many different cases:

1) Arrays are implemented using pointers. Pointers can be used to iterate through an array (as an alternative to array indices) (covered in lesson 9.24 -- Introduction to iterators).

2) They are the only way you can dynamically allocate memory in C++ (covered in lesson 9.13 -- Dynamic memory allocation with new and delete). This is by far the most common use case for pointers.

3) They can be used to pass a function as a parameter to another function (covered in lesson 10.9 -- Function Pointers).

4) They can be used to achieve polymorphism when dealing with inheritance (covered in lesson 18.1 -- Pointers and references to the base class of derived objects).

5) They can be used to have one struct/class point at another struct/class, to form a chain. This is useful in some more advanced data structures, such as linked lists and trees.

So there is actually a surprising number of uses for pointers. But don’t worry if you don’t understand what most of these are yet. Now that you understand what pointers are at a basic level, we can start taking an in-depth look at the various cases in which they’re useful, which we’ll do in subsequent lessons.

## Dynamic memory allocation with new and delete

The need for dynamic memory allocation

C++ supports three basic types of memory allocation, of which you’ve already seen two.

- **Static memory allocation** happens for static and global variables. Memory for these types of variables is allocated once when your program is run and persists throughout the life of your program.

- **Automatic memory allocation** happens for function parameters and local variables. Memory for these types of variables is allocated when the relevant block is entered, and freed when the block is exited, as many times as necessary.

- **Dynamic memory allocation** is the topic of this article.

Both **static** and **automatic allocation** have two things in common:

- The size of the variable / array must be known at compile time.

- Memory allocation and deallocation happens **automatically** (when the variable is instantiated / destroyed).

**Dynamic memory allocation** is a way for running programs to request memory from the operating system when needed. This memory does not come from the program’s limited **stack memory** -- instead, it is allocated from a much larger pool of memory managed by the operating system called the **heap**. On modern machines, the heap can be gigabytes in size.

### Dynamically allocating single variables

```cpp
// To allocate a single variable dynamically, we use the scalar (non-array) form of the new operator:
new int; // dynamically allocate an integer (and discard the result)

// Most often, we’ll assign the return value to our own pointer variable 
// so we can access the allocated memory later.
int *ptr{ new int }; 

// We can then perform indirection through the pointer to access the memory:
*ptr = 7; // assign value of 7 to allocated memory
```

### How does dynamic memory allocation work?

Your computer has memory (probably lots of it) that is available for applications to use. When you run an application, your operating system loads the application into some of that memory. This memory used by your application is divided into different areas, each of which serves a different purpose. One area contains your code. Another area is used for normal operations (keeping track of which functions were called, creating and destroying global and local variables, etc…). We’ll talk more about those later. However, much of the memory available just sits there, waiting to be handed out to programs that request it.

When you dynamically allocate memory, you’re asking the operating system to reserve some of that memory for your program’s use. If it can fulfill this request, it will return the address of that memory to your application. From that point forward, your application can use this memory as it wishes. When your application is done with the memory, it can return the memory back to the operating system to be given to another program.

Unlike static or automatic memory, the program itself is responsible for requesting and disposing of dynamically allocated memory.

```cpp
// Initializing a dynamically allocated variable
int *ptr1{ new int (5) }; // use direct initialization
int *ptr2{ new int { 6 } }; // use uniform initialization

// Deleting single variables
// assume ptr has previously been allocated with operator new
delete ptr; // return the memory pointed to by ptr to the operating system
ptr = 0; // set ptr to be a null pointer (use nullptr instead of 0 in C++11)
```

### Best practices

First, try to avoid having multiple pointers point at the same piece of dynamic memory. If this is not possible, be clear about which pointer “owns” the memory (and is responsible for deleting it) and which are just accessing it.

Second, when you delete a pointer, if that pointer is not going out of scope immediately afterward, set the pointer to 0 (or nullptr in C++11). We’ll talk more about null pointers, and why they are useful in a bit.

Third, set deleted pointers to 0 (or nullptr in C++11) unless they are going out of scope immediately afterward.

Because NULL is a preprocessor macro with an implementation defined value, avoid using NULL. Use nullptr to initialize your pointers to a null value.

In many cases, having new throw an exception (or having your program crash) is undesirable, so there’s an alternate form of new that can be used instead to tell new to return a null pointer if memory can’t be allocated. This is done by adding the constant std::nothrow between the new keyword and the allocation type:

```cpp
int *value = new (std::nothrow) int; // value will be set to a null pointer if the integer allocation fails

int *value{ new (std::nothrow) int{} }; // ask for an integer's worth of memory
if (!value) // handle case where new returned null
{
    // Do error handling here
    std::cout << "Could not allocate memory";
}
```

## Memory leaks

Dynamically allocated memory stays allocated until it is explicitly deallocated or until the program ends (and the operating system cleans it up, assuming your operating system does that). However, the pointers used to hold dynamically allocated memory addresses follow the normal scoping rules for local variables. This mismatch can create interesting problems.

Consider the following function:

```cpp
void doSomething()
{
    int *ptr{ new int{} };
}
```

This function allocates an integer dynamically, but never frees it using delete. Because pointers variables are just normal variables, when the function ends, ptr will go out of scope. And because ptr is the only variable holding the address of the dynamically allocated integer, when ptr is destroyed there are no more references to the dynamically allocated memory. This means the program has now “lost” the address of the dynamically allocated memory. As a result, this dynamically allocated integer can not be deleted.

This is called a **memory leak**. **Memory leaks** happen when your program loses the address of some bit of dynamically allocated memory before giving it back to the operating system. When this happens, your program can’t delete the dynamically allocated memory, because it no longer knows where it is. The operating system also can’t use this memory, because that memory is considered to be still in use by your program.

**Memory leaks** eat up free memory while the program is running, making less memory available not only to this program, but to other programs as well. Programs with severe memory leak problems can **eat all the available memory, causing the entire machine to run slowly or even crash**. Only after your program terminates is the operating system able to clean up and “reclaim” all leaked memory.

Although memory leaks can result from a pointer **going out of scope**, there are other ways that memory leaks can result. For example, a memory leak can occur if a pointer holding the address of the dynamically allocated memory is assigned another value:

```cpp
int value = 5;
int *ptr{ new int{} }; // allocate memory
ptr = &value; // old address lost, memory leak results

// This can be fixed by deleting the pointer before reassigning it:
int value{ 5 };
int *ptr{ new int{} }; // allocate memory
delete ptr; // return memory back to operating system
ptr = &value; // reassign pointer to address of value

// Relatedly, it is also possible to get a memory leak via double-allocation:
int *ptr{ new int{} };
ptr = new int{}; // old address lost, memory leak results
```

## Dynamically allocating arrays

To allocate an array dynamically, we use the array form of new and delete (often called new[] and delete[]):

```cpp
#include <iostream>
#include <cstddef> // std::size_t
 
int main()
{
    std::cout << "Enter a positive integer: ";
    std::size_t length{};
    std::cin >> length;
 
    int *array{ new int[length]{} }; // use array new.  Note that length does not need to be constant!
 
    std::cout << "I just allocated an array of integers of length " << length << '\n';
 
    array[0] = 5; // set element 0 to value 5
 
    delete[] array; // use array delete to deallocate array
 
    // we don't need to set array to nullptr/0 here because it's going to go out of scope immediately after this anyway
 
    return 0;
}
```

## Member selection with pointers and references

```cpp
struct Person
{
    int age{};
    double weight{};
};
 
Person person{};
 
// Member selection using actual struct variable
person.age = 5;
```

This syntax also works for references:

```cpp
struct Person
{
    int age{};
    double weight{};
};
Person person{}; // define a person
 
// Member selection using reference to struct
Person& ref{ person };
ref.age = 5;
```

However, with a pointer, you need to use the arrow operator:

```cpp
struct Person
{
    int age{};
    double weight{};
};
Person person{};
 
// Member selection using pointer to struct
Person* ptr{ &person };
ptr->age = 5;
```

**Rule**

When using a pointer to access the value of a member, use operator-> instead of operator. (the . operator)

The member selection operator is always applied to the currently selected variable. If you have a mix of pointer- and normal member variables, you can see member selections where . and -> are mixed.


```cpp
#include <iostream>
#include <string>
 
struct Paw
{
    int claws{};
};
 
struct Animal
{
    std::string name{};
    Paw paw{};
};
 
int main()
{
    Animal puma{ "Puma", { 5 } };
 
    Animal* pointy{ &puma };
 
    // pointy is a pointer, use ->
    // paw is not a pointer, use .
    std::cout << pointy->paw.claws << '\n';
 
    return 0;
}
```

## An introduction to std::array

std::array provides fixed array functionality that won’t decay when passed into a function. std::array is defined in the ```<array>``` header, inside the std namespace.

```cpp
#include <array>
 
std::array<int, 3> myArray; // declare an integer array with length 3
```

Just like the native implementation of fixed arrays, the length of a std::array must be known at compile time.

```cpp
std::array<int, 5> myArray = { 9, 7, 5, 3, 1 }; // initializer list
std::array<int, 5> myArray2 { 9, 7, 5, 3, 1 }; // list initialization

std::array<int, > myArray { 9, 7, 5, 3, 1 }; // illegal, array length must be provided
std::array<int> myArray { 9, 7, 5, 3, 1 }; // illegal, array length must be provided

// C++17
std::array myArray { 9, 7, 5, 3, 1 }; // The type is deduced to std::array<int, 5>
std::array myArray { 9.7, 7.31 }; // The type is deduced to std::array<double, 2>

// We favor this syntax rather than typing out the type and size at the declaration.
// std::array myArray { 9, 7, 5, 3, 1 }; // Since C++17
std::array<int, 5> myArray { 9, 7, 5, 3, 1 }; // Before C++17
// std::array myArray { 9.7, 7.31 }; // Since C++17
std::array<double, 2> myArray { 9.7, 7.31 }; // Before C++17

// C++20
auto myArray1 { std::to_array<int, 5>({ 9, 7, 5, 3, 1 }) }; // Specify type and size
auto myArray2 { std::to_array<int>({ 9, 7, 5, 3, 1 }) }; // Specify type only, deduce size
auto myArray3 { std::to_array({ 9, 7, 5, 3, 1 }) }; // Deduce type and size

// Accessing std::array values using the subscript operator works just like you would expect:

std::cout << myArray[1] << '\n';
myArray[2] = 6;

myArray.at(1) = 6; // array element 1 is valid, sets array element 1 to value 6
myArray.at(9) = 10; // array element 9 is invalid, will throw an error
```

**Rule**

Always pass std::array by reference or const reference

```cpp
#include <algorithm> // for std::sort
#include <array>
#include <iostream>
 
int main()
{
    std::array<int, 5> myArray { 7, 3, 1, 9, 5 };
    std::sort(myArray.begin(), myArray.end()); // sort the array forwards
//  std::sort(myArray.rbegin(), myArray.rend()); // sort the array backwards
 
    for (int element : myArray)
        std::cout << element << ' ';
 
    std::cout << '\n';
 
    return 0;
}

// This prints:

// 1 3 5 7 9
```

## Array of struct

```cpp
#include <array>
#include <iostream>
 
struct House
{
    int number{};
    int stories{};
    int roomsPerStory{};
};
 
int main()
{
    std::array<House, 3> houses{};
 
    houses[0] = { 13, 4, 30 };
    houses[1] = { 14, 3, 10 };
    houses[2] = { 15, 3, 40 };
 
    for (const auto& house : houses)
    {
        std::cout << "House number " << house.number
                  << " has " << (house.stories * house.roomsPerStory)
                  << " rooms\n";
    }
 
    return 0;
}

// Output

// House number 13 has 120 rooms
// House number 14 has 30 rooms
// House number 15 has 120 rooms

// Doesn't work.
std::array<House, 3> houses{
    { 13, 4, 30 },
    { 14, 3, 10 },
    { 15, 3, 40 }
};
```

** A Reminder

Braces can be omitted during aggregate initialization:

```cpp
struct S
{
  int arr[3]{};
  int i{};
};
 
// These two do the same
S s1{ { 1, 2, 3 }, 4 };
S s2{ 1, 2, 3, 4 };
```

```cpp
#include <iostream>
 
struct House
{
    int number{};
    int stories{};
    int roomsPerStory{};
};
 
struct Array
{
    House value[3]{};
};
 
int main()
{
    // With braces, this works.
    Array houses{
        { { 13, 4, 30 }, { 14, 3, 10 }, { 15, 3, 40 } }
    };
 
    for (const auto& house : houses.value)
    {
        std::cout << "House number " << house.number
                  << " has " << (house.stories * house.roomsPerStory)
                  << " rooms\n";
    }
 
    return 0;
}
```

std::array is a great replacement for built-in fixed arrays. It's efficient, in that it doesn’t use any more memory than built-in fixed arrays. The only real downside of a std::array over a built-in fixed array is a slightly more awkward syntax, that you have to explicitly specify the array length (the compiler won’t calculate it for you from the initializer, unless you also omit the type, which isn't always possible), and the signed/unsigned issues with size and indexing. But those are comparatively minor quibbles — we recommend using std::array over built-in fixed arrays for any non-trivial array use.


## An introduction to std::vector

Unlike std::array, which closely follows the basic functionality of fixed arrays, std::vector comes with some additional tricks up its sleeves. These help make std::vector one of the most useful and versatile tools to have in your C++ toolkit.

Introduced in C++03, std::vector provides dynamic array functionality that handles its own memory management. This means you can create arrays that have their length set at run-time, without having to explicitly allocate and deallocate memory using new and delete. std::vector lives in the ```<vector>``` header.

```cpp
#include <vector>
 
// no need to specify length at the declaration
std::vector<int> array; 
std::vector<int> array2 = { 9, 7, 5, 3, 1 }; // use initializer list to initialize array (Before C++11)
std::vector<int> array3 { 9, 7, 5, 3, 1 }; // use uniform initialization to initialize array
 
// as with std::array, the type can be omitted since C++17
std::vector array4 { 9, 7, 5, 3, 1 }; // deduced to std::vector<int>

// Just like std::array, accessing array elements can be done via the [] operator 
// (which does no bounds checking) or the at() function (which does bounds checking):
array4[3] = 2; // no bounds checking
array4.at(4) = 3; // does bounds checking
```

Note that in both the uninitialized and initialized case, you do not need to include the array length at compile time. This is because std::vector will **dynamically allocate** memory for its contents as requested.

## Self-cleanup prevents memory leaks

```cpp
void doSomething(bool earlyExit)
{
    int *array{ new int[5] { 9, 7, 5, 3, 1 } };
 
    if (earlyExit)
        return;
 
    // do stuff here
 
    delete[] array; // never called
}
```

If earlyExit is set to true, array will never be deallocated, and the memory will be leaked.

However, if array is a std::vector, this won’t happen, because the memory will be deallocated as soon as array goes out of scope (regardless of whether the function exits early or not). This makes std::vector much safer to use than doing your own memory allocation.

## Vectors remember their length

Unlike built-in dynamic arrays, which don’t know the length of the array they are pointing to, std::vector keeps track of its length. We can ask for the vector’s length via the size() function:

```cpp
#include <iostream>
#include <vector>
 
void printLength(const std::vector<int>& array)
{
    std::cout << "The length is: " << array.size() << '\n';
}
 
int main()
{
    std::vector<int> array { 9, 7, 5, 3, 1 };
    printLength(array);
 
    return 0;
}
```

Just like with std::array, size() returns a value of nested type size_type (full type in the above example would be std::vector<int>::size_type), which is an unsigned integer.

## Resizing a vector

```cpp
#include <iostream>
#include <vector>
 
int main()
{
    std::vector array { 0, 1, 2 };
    array.resize(5); // set size to 5
 
    std::cout << "The length is: " << array.size() << '\n';
 
    for (int i : array)
        std::cout << i << ' ';
 
    std::cout << '\n';
 
    return 0;
}
// output
// This prints:

// The length is: 5
// 0 1 2 0 0
```

## Compacting bools

std::vector has another cool trick up its sleeves. There is a special implementation for std::vector of type bool that will compact 8 booleans into a byte! This happens behind the scenes, and doesn’t change how you use the std::vector.

```cpp
#include <vector>
#include <iostream>
 
int main()
{
    std::vector<bool> array { true, false, false, true, true };
    std::cout << "The length is: " << array.size() << '\n';
 
    for (int i : array)
        std::cout << i << ' ';
 
    std::cout << '\n';
 
    return 0;
}

// This prints:

// The length is: 5
// 1 0 0 1 1
```

