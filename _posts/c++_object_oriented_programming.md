---
layout: post
title: "Introduction of C++ Object Oriented Programming (3)"
date: 2021-02-28 00:03:00 +0800
description: "Introduction of C++ Object Oriented Programming (3)" # (optional)
img: 2021-02-28-cover-image-of-cpp-notes.jpg # Add image post (optional)
fig-caption: "Introduction of C++ Object Oriented Programming (3)" # Add figcaption (optional)
tags: ['Programming', 'C++']
categories: ['Programming', 'C++']
---

In the world of object-oriented programming, we often want our types to not only hold data, but provide functions that work with the data as well. In C++, this is typically done via the class keyword. The class keyword defines a new user-defined type called a class.

In C++, classes and structs are essentially the same. In fact, the following struct and class are effectively identical:

```cpp
struct DateStruct
{
    int year{};
    int month{};
    int day{};
};
 
class DateClass
{
public:
    int m_year{};
    int m_month{};
    int m_day{};
};
```

**If reading values with std::cin, it’s a good idea to remove the extraneous newline using std::cin.ignore().**

```cpp
#include <iostream>
#include <string>
 
int main()
{
std::cout << "Pick 1 or 2: ";
int choice{};
std::cin >> choice;
 
std::cin.ignore(32767, '\n'); // ignore up to 32767 characters until a \n is removed
 
std::cout << "Now enter your name: ";
std::string name{};
std::getline(std::cin, name);
 
std::cout << "Hello, " << name << ", you picked " << choice << '\n';
 
return 0;
}
```

### What’s that 32767 magic number in your code?

That tells std::cin.ignore() how many characters to ignore up to. We picked that number because it’s the largest signed value guaranteed to fit in a (2-byte) integer on all platforms.

Technically, the correct way to ignore an unlimited amount of input is as follows:

```cpp
#include <limits>
 
...
 
std::cin.ignore(std::numeric_limits<std::streamsize>::max(), '\n'); // ignore unlimited characters until a \n is removed
```

## Enumerated types

An enumerated type (also called an enumeration or enum) is a data type where every possible value is defined as a symbolic constant (called an enumerator). Enumerations are defined via the enum keyword.

```cpp
// Define a new enumeration named Color
enum Color
{
    // Here are the enumerators
    // These define all the possible values this type can hold
    // Each enumerator is separated by a comma, not a semicolon
    color_black,
    color_red,
    color_blue,
    color_green,
    color_white,
    color_cyan,
    color_yellow,
    color_magenta, // there can be a comma after the last enumerator, but there doesn't have to be a comma
}; // however the enum itself must end with a semicolon
 
// Define a few variables of enumerated type Color
Color paint = color_white;
Color house(color_blue);
Color apple { color_red };
```

Defining an enumeration (or any user-defined data type) does not allocate any memory. When a variable of the enumerated type is defined (such as variable paint in the example above), memory is allocated for that variable at that time.

Because enumerators are placed into the same namespace as the enumeration, an enumerator name can’t be used in multiple enumerations within the same namespace:

```cpp
enum Color
{
  red,
  blue, // blue is put into the global namespace
  green
};
 
enum Feeling
{
  happy,
  tired,
  blue // error, blue was already used in enum Color in the global namespace
};
```

Consequently, it’s common to prefix enumerators with a standard prefix like animal_ or color_, both to prevent naming conflicts and for code documentation purposes.

Each enumerator is automatically assigned an integer value based on its position in the enumeration list. By default, the first enumerator is assigned the integer value 0, and each subsequent enumerator has a value one greater than the previous enumerator:

```cpp
enum Color
{
    color_black, // assigned 0
    color_red, // assigned 1
    color_blue, // assigned 2
    color_green, // assigned 3
    color_white, // assigned 4
    color_cyan, // assigned 5
    color_yellow, // assigned 6
    color_magenta // assigned 7
};
 
Color paint{ color_white };
std::cout << paint;

Color color{};
std::cin >> color; // will cause compiler error
```

It is possible to explicitly define the value of enumerator. These integer values can be positive or negative and can share the same value as other enumerators. Any non-defined enumerators are given a value one greater than the previous enumerator.

```cpp
// define a new enum named Animal
enum Animal
{
    animal_cat = -3,
    animal_dog, // assigned -2
    animal_pig, // assigned -1
    animal_horse = 5,
    animal_giraffe = 5, // shares same value as animal_horse
    animal_chicken // assigned 6
};
```

Note in this case, animal_horse and animal_giraffe have been given the same value. When this happens, the enumerations become non-distinct -- essentially, animal_horse and animal_giraffe are interchangeable. Although C++ allows it, assigning the same value to two enumerators in the same enumeration should generally be avoided.

### Best practice

**Don’t assign specific values to your enumerators.**

**Don’t assign the same value to two enumerators in the same enumeration unless there’s a very good reason.**

If you want to use a different integer type for enumerators, for example to save bandwidth when networking an enumerator, you can specify it at the enum declaration.

```cpp
// Use an 8 bit unsigned integer as the enum base.
enum Color : std::uint_least8_t
{
    color_black,
    color_red,
    // ...
};
```

Since enumerators aren’t usually used for arithmetic or comparisons, it’s safe to use an unsigned integer. We also need to specify the enum base when we want to forward declare an enum.

```cpp
enum Color; // Error
enum Color : int; // Okay
 
// ...
 
// Because Color was forward declared with a fixed base, we
// need to specify the base again at the definition.
enum Color : int
{
    color_black,
    color_red,
    // ...
};
```

```cpp
#include <iostream>
 
int main()
{
    enum Color
    {
        color_red, // color_red is placed in the same scope as Color
        color_blue
    };
 
    enum Fruit
    {
        fruit_banana, // fruit_banana is placed in the same scope as Fruit
        fruit_apple
    };
	
    Color color{ color_red }; // Color and color_red can be accessed in the same scope (no prefix needed)
    Fruit fruit{ fruit_banana }; // Fruit and fruit_banana can be accessed in the same scope (no prefix needed)
 
    if (color == fruit) // The compiler will compare a and b as integers
        std::cout << "color and fruit are equal\n"; // and find they are equal!
    else
        std::cout << "color and fruit are not equal\n";
 
    return 0;
}
```

With normal enumerations, enumerators are placed in the same scope as the enumeration itself, so you can typically access enumerators directly (e.g. red). However, with enum classes, the strong scoping rules mean that all enumerators are considered part of the enumeration, so you have to use a scope qualifier to access the enumerator (e.g. Color::red). This helps keep name pollution and the potential for name conflicts down.

Because the enumerators are part of the enum class, there’s no need to prefix the enumerator names (e.g. it’s okay to name them “red” instead of “color_red”, since Color::color_red is redundant).

The strong typing rules means that each enum class is considered a unique type. This means that the compiler will not implicitly compare enumerators from different enumerations. If you try to do so, the compiler will throw an error, as shown in the example above.

However, note that you can still compare enumerators from within the same enum class (since they are of the same type):

```cpp
#include <iostream>
 
int main()
{
    enum class Color
    {
        red,
        blue
    };
 
    Color color{ Color::red };
 
    if (color == Color::red) // this is okay
        std::cout << "The color is red!\n";
    else if (color == Color::blue)
        std::cout << "The color is blue!\n";
 
    return 0;
}
```

## Structs

 A struct (short for structure) allows us to group variables of mixed data types together into a single unit.

 Because structs are user-defined, we first have to tell the compiler what our struct looks like before we can begin using it. To do this, we declare our struct using the struct keyword. Here is an example of a struct declaration:

```cpp
struct Employee
{
    int id{};
    int age{};
    double wage{};
};

// In order to use the Employee struct, we simply declare a variable of type Employee:
Employee joe{}; // struct Employee is capitalized, variable joe is not

// It is possible to define multiple variables of the same struct type:
joe.id = 14; // assign a value to member id within struct joe
joe.age = 32; // assign a value to member age within struct joe
joe.wage = 24.15; // assign a value to member wage within struct joe

Employee frank{}; // create an Employee struct for Frank
frank.id = 15; // assign a value to member id within struct frank
frank.age = 28; // assign a value to member age within struct frank
frank.wage = 18.27; // assign a value to member wage within struct frank
```

### Initializing structs

Initializing structs by assigning values member by member is a little cumbersome, so C++ supports a faster way to initialize structs using an initializer list. This allows you to initialize some or all the members of a struct at declaration time.

```cpp
struct Employee
{
    int id{};
    int age{};
    double wage{};
};
 
Employee joe{ 1, 32, 60000.0 }; // joe.id = 1, joe.age = 32, joe.wage = 60000.0
Employee frank{ 2, 28 }; // frank.id = 2, frank.age = 28, frank.wage = 0.0 (default initialization)
```

### Non-static member initialization

It’s possible to give non-static (normal) struct members a default value:

```cpp
struct Rectangle
{
    double length{ 1.0 };
    double width{ 1.0 };
};
 
int main()
{
    Rectangle x{}; // length = 1.0, width = 1.0
 
    x.length = 2.0; // you can assign other values like normal
 
    return 0;
}
```

If both non-static member initializer and list-initialization are provided, the list-initialization takes precedence.

```cpp
struct Rectangle
{
    double length{ 1.0 };
    double width{ 1.0 };
};
 
int main()
{
    Rectangle x{ 2.0, 2.0 };
 
    return 0;
}
```

In the above example, Rectangle x would be initialized with length and width 2.0.

### Structs and functions

A big advantage of using **structs** over **individual variables** is that we can pass the entire struct to a function that needs to work with the members:

```cpp
#include <iostream>
 
struct Employee
{
    int id{};
    int age{};
    double wage{};
};
 
void printInformation(Employee employee)
{
    std::cout << "ID:   " << employee.id << '\n';
    std::cout << "Age:  " << employee.age << '\n';
    std::cout << "Wage: " << employee.wage << '\n';
}
 
int main()
{
    Employee joe { 14, 32, 24.15 };
    Employee frank { 15, 28, 18.27 };
 
    // Print Joe's information
    printInformation(joe);
 
    std::cout << '\n';
 
    // Print Frank's information
    printInformation(frank);
 
    return 0;
}
```

### Nested structs

```cpp
struct Employee
{
    int id{};
    int age{};
    double wage{};
};
 
struct Company
{
    Employee CEO{}; // Employee is a struct within the Company struct
    int numberOfEmployees{};
};
 
Company myCompany{{ 1, 42, 60000.0 }, 5 };
```

### Accessing structs across multiple files

Because struct declarations do not take any memory, if you want to share a struct declaration across multiple files (so you can instantiate variables of that struct type in multiple files), put the struct declaration in a header file, and #include that header file anywhere you need it.

Struct variables are subject to the same rules as normal variables. Consequently, to make a struct variable accessible across multiple files, you can use the extern keyword in the declaration in the header and define the variable in a source file.

### Final notes on structs

Structs are very important in C++, as understanding structs is the first major step towards object-oriented programming! Later on in these tutorials, you’ll learn about another aggregate data type called a class, which is built on top of structs. Understanding structs well will help make the transition to classes that much easier.

The structs introduced in this lesson are sometimes called plain old data structs (or POD structs) since the members are all data (variable) members. In the future (when we discuss classes) we’ll talk about other kinds of members.