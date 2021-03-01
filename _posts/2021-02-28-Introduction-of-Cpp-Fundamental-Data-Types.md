---
layout: post
title: "Introduction of C++ Fundamental Data Types (1)"
date: 2021-02-28 00:01:00 +0800
description: "Introduction of C++ Fundamental Data Types (1)" # (optional)
img: 2021-02-28-cover-image-of-cpp-notes.jpg # Add image post (optional)
fig-caption: "Introduction of C++ Fundamental Data Types (1)" # Add figcaption (optional)
tags: ['Programming', 'C++']
categories: ['Programming', 'C++']
---

## C++ fundamental data types

In C++, direct memory access is not allowed. Instead, we access memory indirectly through an **object**. An object is a region of storage (usually memory) that has a value and other associated properties (that we’ll cover in future lessons). When an object is defined, the compiler automatically determines where the object will be placed in memory. As a result, rather than say go get the value stored in mailbox number 7532, we can say, go get the value stored by this object and the compiler knows where in memory to look for that value. This means we can focus on using objects to store and retrieve values, and not have to worry about where in memory they’re actually being placed.

**Objects** can be named or unnamed (anonymous). **A named object** is called a **variable**, and the name of the object is called an identifier. In our programs, most of the objects we create will be **variables**.

When the program is run (called runtime), **the variable will be instantiated**. Instantiation is a fancy word that means the object will be **created** and **assigned a memory address**. Variables must be instantiated before they can be used to store values. For the sake of example, let’s say that variable x is instantiated at memory location 140. Whenever the program then uses variable x, it will access the value in memory location 140. An instantiated object is sometimes also called an instance.

## Headers

Header files allow us to put declarations in one location and then import them wherever we need them. This can save a lot of typing in multi-file programs. When you #include a file, the content of the included file is inserted at the point of inclusion. This provides a useful way to pull in declarations from another file.

### Best practice

- Header files should generally not contain function and variable definitions, so as not to violate the one definition rule. An exception is made for symbolic constants (which we cover in lesson 4.13 -- Const, constexpr, and symbolic constants).

- Use a .h suffix when naming your header files.

- If a header file is paired with a code file (e.g. add.h with add.cpp), they should both have the same base name (add).

- When writing a source file, include the corresponding header (If one exists), even if you don’t need it yet.

- Use double quotes to include header files that you’ve written or are expected to be found in the current directory. Use angled brackets to include headers that come with your compiler, OS, or third-party libraries you’ve installed elsewhere on your system.

- When including a header file from the standard library, use the no extension version (without the .h) if it exists. User-defined headers should still use a .h extension.

- Each file should explicitly #include all of the header files it needs to compile. Do not rely on headers included transitively from other headers.

- Order your #includes as follows: your own user-defined headers first, then 3rd party library headers, then standard library headers, with the headers in each section sorted alphabetically.

### Header Recommendations 

- Always include header guards (we’ll cover these next lesson).

- Do not define variables and functions in header files (global constants are an exception -- we’ll cover these later)

- Give your header files the same name as the source files they’re associated with (e.g. grades.h is paired with grades.cpp).

- Each header file should have a specific job, and be as independent as possible. For example, you might put all your declarations related to functionality A in A.h and all your declarations related to functionality B in B.h. That way if you only care about A later, you can just include A.h and not get any of the stuff related to B.

- Be mindful of which headers you need to explicitly include for the functionality that you are using in your code files

- Every header you write should compile on its own (it should #include every dependency it needs)

- Only #include what you need (don’t include everything just because you can).

- Do not #include .cpp files.

## Header guards

The good news is that we can avoid the above problem via a mechanism called a header guard (also called an include guard). Header guards are conditional compilation directives that take the following form:

```cpp
#ifndef SOME_UNIQUE_NAME_HERE
#define SOME_UNIQUE_NAME_HERE
 
// your declarations (and certain types of definitions) here
 
#endif
```

All of your header files should have header guards on them. SOME_UNIQUE_NAME_HERE can be any name you want, but by convention is set to the full filename of the header file, typed in all caps, using underscores for spaces or punctuation. For example, square.h would have the header guard:

```cpp
//square.h

#ifndef SQUARE_H
#define SQUARE_H
 
int getSquareSides()
{
    return 4;
}
 
#endif
```

Note that the goal of header guards is to **prevent a code file from receiving more than one copy of a guarded header**. By design, header guards **do not** prevent a given header file from being included (once) into **separate code files**. This can also cause unexpected problems. Consider:

## \#pragma once

Many compilers support a simpler, alternate form of header guards using the #pragma directive:

```cpp
#pragma once
 
// your code here
```

\#pragma once serves the same purpose as header guards, and has the added benefit of being shorter and less error-prone.

However, #pragma once is **not** an official part of the C++ language, and not all compilers support it (although most modern compilers do).

## Fundamental data types

The smallest unit of memory is a **binary digit** (also called a **bit**), which can hold a value of 0 or 1.

**Memory** is organized into sequential units called memory addresses (or addresses for short). 

A byte is a group of bits that are operated on as a unit. The modern standard is that a byte is comprised of 8 sequential bits. In C++, we typically work with “byte-sized” chunks of data.

|	Types	|	Category	|	Meaning	|	Example	|
|:---|:---|:---|:---|
|float<br />double<br />long double<br />|Floating Point|a number with a fractional part|3.14159|
|bool|Integral (Boolean)|true or false|true|
|char<br />wchar_t<br />char8_t (C++20)<br />char16_t (C++11)<br />char32_t (C++11)|Integral (Character)|a single character of text|'c'|
|short<br />int<br />long<br />long long (C++11)|Integral (Integer)|positive and negative whole numbers, including 0|64|
|std::nullptr_t (C++11)|Null Pointer|a null pointer|nullptr|
|void|Void|no type|n/a|

> The terms integer and integral are similar, but have different meanings. An integer is a specific data type that hold non-factional numbers, such as whole numbers, 0, and negative whole numbers. Integral means “like an integer”. Most often, integral is used as part of the term integral type, which includes all of the Boolean, characters, and integer types (also enumerated types, which we’ll discuss in chapter 8). Integral type are named so because they are stored in memory as integers, even though their behaviors might vary (which we’ll see later in this chapter when we talk about the character types).

## The \_t suffix

Many of the types defined in newer versions of C++ (e.g. std::nullptr_t) use a \_t suffix. This suffix means “type”, and it’s a common nomenclature applied to modern types.

If you see something with a \_t suffix, it’s probably a type. But many types don’t have a \_t suffix, so this isn’t consistently applied.

## Void

Void is the easiest of the data types to explain. Basically, void means “no type”!

### Best practice

Use an empty parameter list instead of void to indicate that a function has no parameters.

## Fundamental data type sizes

|	Category	|	Type	|	Minimum Size	|	Note	|
|:---|:---|:---|:---|
|boolean|bool|1 byte||
|character|char|1 byte|Always exactly 1 byte|
||wchar_t|1 byte|'c'|
||char16_t|2 byte|C++11 type|
||char32_t|4 bytes|C++11 type|
|integer|short|2 bytes||
||int|2 bytes||
||long|4 bytes||
||long long|8 bytes|C99/C++11 type|
|floating point|float|4 bytes||
||double|8 bytes||
||long double|8 bytes||

### Best practice

For maximum compatibility, you shouldn’t assume that variables are larger than the specified minimum size.

## Signed integers

An integer is an integral type that can represent positive and negative whole numbers, including 0 (e.g. -2, -1, 0, 1, 2). C++ has 4 different fundamental integer types available for use:

|	Type	|	Minimum Size	|	Note	|
|:---|:---|:---|
|short|16 bits|1 byte|
|int|16 bits|Typically 32 bits on modern architectures|
|long|32 bits|1 byte|
|long long|64 bits|1 byte|

Here’s a table containing the range of signed integers of different sizes:

|	Size/Type	|	Range	|
|:---|:---|
|8 bit signed|-2^7~2^7 -128 to 127|
|16 bit signed|-2^15~2^15 -32,768 to 32,767|
|32 bit signed|-2^31~2^31 -2,147,483,648 to 2,147,483,647|
|64 bit signed|-2^63~2^63 -9,223,372,036,854,775,808 to 9,223,372,036,854,775,807|

For the math inclined, an n-bit signed variable has a range of -(2n-1) to 2n-1-1.

By default, integers are signed, which means the number’s sign is preserved. Therefore, a signed integer can hold both positive and negative numbers (and 0).

### A reminder

C++ only guarantees that integers will have a certain minimum size, not that they will have a specific size. 

### Best practice

Prefer the shorthand types that do not use the int suffix or signed prefix.

## Unsigned integers

Unsigned integers are integers that can only hold non-negative whole numbers, including 0.

Unsigned integer range

|	Size/Type	|	Range	|
|:---|:---|
|1 byte unsigned|2^8 0 to 255|
|2 byte unsigned|2^16 0 to 65,535|
|4 byte unsigned|2^32 0 to 4,294,967,295|
|8 byte unsigned|2^64 0 to 18,446,744,073,709,551,615|

> Oddly, the C++ standard explicitly says “a computation involving unsigned operands can never overflow”. This is contrary to general programming consensus that integer overflow encompasses both signed and unsigned use cases. Given that most programmers would consider this overflow, we’ll call this overflow despite C++’s statements to the contrary.

### Integer best practices

Now that fixed-width integers have been added to C++, the best practice for integers in C++ is as follows:

- int should be preferred when the size of the integer doesn’t matter (e.g. the number will always fit within the range of a 2 byte signed integer). For example, if you’re asking the user to enter their age, or counting from 1 to 10, it doesn’t matter whether int is 16 or 32 bits (the numbers will fit either way). This will cover the vast majority of the cases you’re likely to run across.

- If you need a variable guaranteed to be a particular size and want to favor performance, use std::int_fast#\_t.

- If you need a variable guaranteed to be a particular size and want to favor memory conservation over performance, use std::int_least#\_t. This is used most often when allocating lots of variables.

Avoid the following if possible:

- Unsigned types, unless you have a compelling reason.

- The 8-bit fixed-width integer types.

- Any compiler-specific fixed-width integers -- for example, Visual Studio defines \_\_int8, \_\_int16, etc…

## Sscientific Notation

> Scientific notation is a useful shorthand for writing lengthy numbers in a concise manner. And although scientific notation may seem foreign at first, understanding scientific notation will help you understand how floating point numbers work, and more importantly, what their limitations are.

Numbers in scientific notation take the following form: significand x 10exponent. For example, in the scientific notation 1.2 x 10<sup>4</sup>, 1.2 is the significand and 4 is the exponent. Since 10<sup>4</sup> evaluates to 10,000, 1.2 x 10<sup>4</sup> evaluates to 12,000.

By convention, numbers in scientific notation are written with one digit before the decimal point, and the rest of the digits afterward.

## Floating point numbers

Integers are great for counting whole numbers, but sometimes we need to store very large numbers, or numbers with a fractional component. A floating point type variable is a variable that can hold a real number, such as 4320.0, -3.33, or 0.01226. The floating part of the name floating point refers to the fact that the decimal point can “float”; that is, it can support a variable number of digits before and after the decimal point.

There are three different floating point data types: float, double, and long double. As with integers, C++ does not define the actual size of these types (but it does guarantee minimum sizes). On modern architectures, floating point representation almost always follows IEEE 754 binary format. In this format, a float is 4 bytes, a double is 8, and a long double can be equivalent to a double (8 bytes), 80-bits (often padded to 12 bytes), or 16 bytes.

Floating point data types are always signed (can hold positive and negative values).

|	Category	|	Type	|	Minimum Size	|	Typical Size	|
|:---|:---|:---|:---|
|floating point|float|4 bytes|4 bytes|
||double|8 bytes|8 bytes|
||long double|8 bytes|8, 12, or 16 bytes|

Assuming IEEE 754 representation:

|	Size	|	Range	|	Precision	|
|4 bytes	|±1.18 x 10<sup>-38</sup> to ±3.4 x 10<sup>38</sup>	|6-9 significant digits, typically 7
|8 bytes	|±2.23 x 10<sup>-308</sup> to ±1.80 x 10<sup>308</sup>	|15-18 significant digits, typically 16
|80-bits (typically uses 12 or 16 bytes)	|±3.36 x 10<sup>-4932</sup> to ±1.18 x 10<sup>4932</sup>	|18-21 significant digits
|16 bytes	|±3.36 x 10<sup>-4932</sup> to ±1.18 x 10<sup>4932</sup>	|33-36 significant digits

### Best practice

Always make sure the type of your literals match the type of the variables they’re being assigned to or used to initialize. Otherwise an unnecessary conversion will result, possibly with a loss of precision.

The **precision** of a floating point number defines how many significant digits it can represent without information loss.

```cpp
#include <iomanip> // for std::setprecision()
#include <iostream>
 
int main()
{
    std::cout << std::setprecision(16); // show 16 digits of precision
    std::cout << 3.33333333333333333333333333333333333333f <<'\n'; // f suffix means float
    std::cout << 3.33333333333333333333333333333333333333 << '\n'; // no suffix means double
    return 0;
}
// OUTPUT
// 3.333333253860474
// 3.333333333333334
```

### Best practice

Favor double over float unless space is at a premium, as the lack of precision in a float will often lead to inaccuracies.

Avoid division by 0 altogether, even if your compiler supports it.

### Rounding errors make floating point comparisons tricky

Floating point numbers are tricky to work with due to non-obvious differences between binary (how data is stored) and decimal (how we think) numbers. Consider the fraction 1/10. In decimal, this is easily represented as 0.1, and we are used to thinking of 0.1 as an easily representable number with 1 significant digit. However, in binary, 0.1 is represented by the infinite sequence: 0.00011001100110011… Because of this, when we assign 0.1 to a floating point number, we’ll run into precision problems.

```cpp
#include <iomanip> // for std::setprecision()
#include <iostream>
 
int main()
{
    double d{0.1};
    std::cout << d << '\n'; // use default cout precision of 6
    std::cout << std::setprecision(17);
    std::cout << d << '\n';

    std::cout << std::setprecision(17);
    double d1{ 1.0 };
    std::cout << d1 << '\n';
    double d2{ 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 + 0.1 }; // should equal 1.0
    std::cout << d2 << '\n';

    return 0;
}
// OUTPUT
// 0.1
// 0.10000000000000001
// 1
// 0.99999999999999989

```

## Boolean values

Boolean variables are variables that can have only two possible values: true, and false.

```cpp
#include <iostream>
 
int main()
{
    std::cout << true << '\n'; // true evaluates to 1
    std::cout << !true << '\n'; // !true evaluates to 0
 
    bool b{false};
    std::cout << b << '\n'; // b is false, which evaluates to 0
    std::cout << !b << '\n'; // !b is true, which evaluates to 1

    std::cout << std::boolalpha; // print bools as true or false
    std::cout << true << '\n';
    std::cout << false << '\n';

    return 0;
}
// OUTPUT
// 1
// 0
// 0
// 1
// true
// false
```

## Chars

The char data type is an integral type, meaning the underlying value is stored as an integer, and it’s guaranteed to be 1-byte in size. However, similar to how a boolean value is interpreted as true or false, a char value is interpreted as an ASCII character.

ASCII stands for American Standard Code for Information Interchange, and it defines a particular way to represent English characters (plus a few other symbols) as numbers between 0 and 127 (called an ASCII code or code point). For example, ASCII code 97 is interpreted as the character ‘a’.

Character literals are always placed between single quotes.

|Code	|Symbol	|Code	|Symbol	|Code	|Symbol	|Code	|Symbol|
|:-----|:-----|:-----|:-----|:-----|:-----|:-----|:-----|
|0	|NUL (null)	|32	|(space)	|64	|@	|96	|\`|
|1	|SOH (start of header)	|33	|!	|65	|A	|97	|a|
|2	|STX (start of text)	|34	|”	|66	|B	|98	|b|
|3	|ETX (end of text)	|35	|#	|67	|C	|99	|c|
|4	|EOT (end of transmission)	|36	|$	|68	|D	|100	|d|
|5	|ENQ (enquiry)	|37	|%	|69	|E	|101	|e|
|6	|ACK (acknowledge)	|38	|&	|70	|F	|102	|f|
|7	|BEL (bell)	|39	|’	|71	|G	|103	|g|
|8	|BS (backspace)	|40	|(	|72	|H	|104	|h|
|9	|HT (horizontal tab)	|41	|)	|73	|I	|105	|i|
|10	|LF (line feed/new line)	|42	|*	|74	|J	|106	|j|
|11	|VT (vertical tab)	|43	|+	|75	|K	|107|	k|
|12	|FF (form feed / new page)	|44	|,	|76	|L	|108	|l|
|13	|CR (carriage return)	|45	|-	|77	|M	|109	|m|
|14	|SO (shift out)	|46	|.	|78	|N	|110	|n|
|15	|SI (shift in)	|47	|/	|79|	O	|111	|o|
|16	|DLE (data link escape)	|48	|0	|80	|P	|112	|p|
|17	|DC1 (data control 1)	|49	|1	|81	|Q	|113	|q|
|18	|DC2 (data control 2)	|50	|2	|82	|R	|114	|r|
|19	|DC3 (data control 3)	|51	|3	|83	|S	|115	|s|
|20	|DC4 (data control 4)	|52	|4	|84	|T	|116	|t|
|21	|NAK (negative acknowledge)	|53	|5	|85	|U	|117	|u|
|22	|SYN (synchronous idle)|	54	|6	|86	|V	|118	|v|
|23	|ETB (end of transmission block)	|55	|7	|87	|W	|119	|w|
|24	|CAN (cancel)	|56	|8	|88	|X	|120	|x|
|25	|EM (end of medium)	|57	|9	|89	|Y	|121	|y|
|26	|SUB (substitute)	|58	|:	|90	|Z	|122	|z|
|27	|ESC (escape)	|59	|;	|91	|[	|123	|\{|
|28	|FS (file separator)	|60	|<	|92	|\\	|124	|\||
|29	|GS (group separator)	|61	|=	|93	|]	|125	|\}|
|30	|RS (record separator)	|62	|>	|94	|^	|126|~|
|31	|US (unit separator)	|63	|?	|95	|_	|127	|DEL (delete)|

Whenever you see C++ syntax (excluding the preprocessor) that makes use of angled brackets, the thing between the angled brackets will most likely be a type. This is typically how C++ deals with concepts that need a parameterizable type.

```cpp 
int main()
{
    std::cout << "Input a keyboard character: ";
 
    char ch{};
    std::cin >> ch;
    std::cout << ch << " has ASCII code " << static_cast<int>(ch) << '\n';
 
    return 0;
}

// OUTPUT
// Input a keyboard character: q
// q has ASCII code 113
```

Here’s a table of all of the escape sequences:

|Name	|Symbol	|Meaning|
|:------|:------|:------|
|Alert	|\\a	|Makes an alert, such as a beep|
|Backspace	|\\b	|Moves the cursor back one space|
|Formfeed	|\\f	|Moves the cursor to next logical page|
|Newline	|\\n	|Moves cursor to next line|
|Carriage return	|\\r	|Moves cursor to beginning of line|
|Horizontal tab	|\\t	|Prints a horizontal tab|
|Vertical tab	|\\v	|Prints a vertical tab|
|Single quote	|\\’	|Prints a single quote|
|Double quote	|\\”	|Prints a double quote|
|Backslash	|\\\\	|Prints a backslash.|
|Question mark	|\\?	|Prints a question mark.<br />No longer relevant. You can use question marks unescaped.|
|Octal number	|\\(number)	|Translates into char represented by octal|
|Hex number	|\\x(number)	|Translates into char represented by hex number|

## Literals

In programming, a constant is a fixed value that may not be changed. C++ has two kinds of constants: literal constants, and symbolic constants. We’ll cover literal constants in this lesson, and symbolic constants in the next lesson.

Literal constants (usually just called literals) are values inserted directly into the code.

```cpp
#include <iostream>
 
int main()
{
    int bin{};
    bin = 0x01; // assign binary 0000 0001 to the variable
    bin = 0x02; // assign binary 0000 0010 to the variable
    bin = 0x04; // assign binary 0000 0100 to the variable
    bin = 0x08; // assign binary 0000 1000 to the variable
    bin = 0x10; // assign binary 0001 0000 to the variable
    bin = 0x20; // assign binary 0010 0000 to the variable
    bin = 0x40; // assign binary 0100 0000 to the variable
    bin = 0x80; // assign binary 1000 0000 to the variable
    bin = 0xFF; // assign binary 1111 1111 to the variable
    bin = 0xB3; // assign binary 1011 0011 to the variable
    bin = 0xF770; // assign binary 1111 0111 0111 0000 to the variable
 
    return 0;
}
```

## Const, constexpr, and symbolic constants

```cpp
const double gravity { 9.8 };
gravity = 9.9; // not allowed, this will cause a compile error

std::cout << "Enter your age: ";
int age{};
std::cin >> age;
 
const int usersAge { age }; // usersAge can not be changed
```

C++ actually has two different kinds of constants.

**Runtime constants** are those whose initialization values can only be resolved at runtime (when your program is running). Variables such as _usersAge_ in the snippets above are runtime constants, because the compiler can’t determine their initial values at compile time. usersAge relies on user input (which can only be given at runtime) and myValue depends on the value passed into the function (which is only known at runtime). However, once initialized, the value of these constants can’t be changed.

**Compile-time constants** are those whose initialization values can be resolved at compile-time (when your program is compiling). Variable _gravity_ above is an example of a compile-time constant. Compile-time constants enable the compiler to perform optimizations that aren’t available with runtime constants. For example, whenever gravity is used, the compiler can simply substitute the identifier gravity with the literal double 9.8.

When you declare a const variable, the compiler will implicitly keep track of whether it’s a runtime or compile-time constant.

## constexpr

To help provide more specificity, C++11 introduced the keyword constexpr, which ensures that a constant must be a **compile-time constant**:

```cpp
constexpr double gravity { 9.8 }; // ok, the value of 9.8 can be resolved at compile-time
constexpr int sum { 4 + 5 }; // ok, the value of 4 + 5 can be resolved at compile-time
 
std::cout << "Enter your age: ";
int age{};
std::cin >> age;
constexpr int myAge { age }; // not okay, age can not be resolved at compile-time
```

constexpr variables are const. 

### Best practices

- Any variable that should not be modifiable after initialization and whose initializer is known at compile-time should be declared as constexpr.

- Any variable that should not be modifiable after initialization and whose initializer is not known at compile-time should be declared as const.

- Use constexpr variables to provide a name and context for your magic numbers.