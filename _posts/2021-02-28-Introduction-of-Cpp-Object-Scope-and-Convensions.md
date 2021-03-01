---
layout: post
title: "Introduction of C++ Object Scope and Convensions (2)"
date: 2021-02-28 00:02:00 +0800
description: "Introduction of C++ Object Scope and Convensions (2)" # (optional)
img: 2021-02-28-cover-image-of-cpp-notes.jpg # Add image post (optional)
fig-caption: "Introduction of C++ Object Scope and Convensions (2)" # Add figcaption (optional)
tags: ['Programming', 'C++']
categories: ['Programming', 'C++']
---

> A **compound statement** (also called a block, or block statement) is a group of zero or more statements that is treated by the compiler as if it were a single statement.

Blocks begin with a { symbol, end with a } symbol, with the statements to be executed being placed in between. Blocks can be used anywhere a single statement is allowed. No semicolon is needed at the end of a block.

```cpp
int add(int x, int y)
{ // block
    return x + y;
} // end block
 
int main()
{ // outer block
 
    // multiple statements
    int value {};  // this is initialization, not a block
 
    { // inner/nested block
        add(3, 4);
    } // end inner/nested block
 
    return 0;
 
} // end outer block

```

When blocks are nested, the enclosing block is typically called the outer block and the enclosed block is called the inner block or nested block.

## User Defined Namespaces

C++ allows us to define our own namespaces via the namespace keyword. Namespaces that you create for your own declarations are called user-defined namespaces. Namespaces provided by C++ (such as the global namespace) or by libraries (such as namespace std) are not considered user-defined namespaces.

Namespace identifiers are typically non-capitalized.

```cpp
// foo.cpp
namespace foo // define a namespace named foo
{
    // This doSomething() belongs to namespace foo
    int doSomething(int x, int y)
    {
        return x + y;
    }
}

// goo.cpp
namespace goo // define a namespace named goo
{
    // This doSomething() belongs to namespace goo
    int doSomething(int x, int y)
    {
        return x - y;
    }
}

int doSomething(int x, int y)
{
    return x * y;
}

int main()
{
    std::cout << foo::doSomething(4, 3) << '\n'; // use the doSomething() that exists in namespace foo
    std::cout << ::doSomething(4, 3) << '\n'; // use the doSomething()
    return 0;
}
```

### Nested namespaces

Namespaces can be nested inside other namespaces. For example:

```cpp
#include <iostream>
 
namespace foo
{
    namespace goo // goo is a namespace inside the foo namespace
    {
        int add(int x, int y)
        {
            return x + y;
        }
    }
}
 
int main()
{
    std::cout << foo::goo::add(1, 2) << '\n';
    return 0;
}
```

Note that because namespace goo is inside of namespace foo, we access add as foo::goo::add.

Since C++17, nested namespaces can also be declared this way:

```cpp
#include <iostream>
 
namespace foo::goo // goo is a namespace inside the foo namespace (C++17 style)
{
  int add(int x, int y)
  {
    return x + y;
  }
}
 
int main()
{
    std::cout << foo::goo::add(1, 2) << '\n';
    return 0;
}
```

### When you should use namespaces

In applications, namespaces can be used to separate application-specific code from code that might be reusable later (e.g. math functions). For example, physical and math functions could go into one namespace (e.g. math::). Language and localization functions in another (e.g. lang::).

When you write a library or code that you want to distribute to others, always place your code inside a namespace. The code your library is used in may not follow best practices -- in such a case, if your library’s declarations aren’t in a namespace, there’s an elevated chance for naming conflicts to occur. As an additional advantage, placing library code inside a namespace also allows the user to see the contents of your library by using their editor’s auto-complete and suggestion feature.

## Global Variables

In C++, variables can also be declared outside of a function. Such variables are called global variables.

```cpp
#include <iostream>
 
// Variables declared outside of a function are global variables
int g_x {}; // global variable g_x
 
void doSomething()
{
    // global variables can be seen and used everywhere in the file
    g_x = 3;
    std::cout << g_x << '\n';
}
 
int main()
{
    doSomething();
    std::cout << g_x << '\n';
 
    // global variables can be seen and used everywhere in the file
    g_x = 5;
    std::cout << g_x << '\n';
 
    return 0;
}
// g_x goes out of scope here
// 3
// 3
// 5
```

### Best practice

Consider using a “g” or “g_” prefix for global variables to help differentiate them from local variables.

## Global variables have file scope and static duration

Global variables have file scope (also informally called global scope or global namespace scope), which means they are visible from the point of declaration until the end of the file in which they are declared. Once declared, a global variable can be used anywhere in the file from that point onward! In the above example, global variable g_x is used in both functions doSomething() and main().

### Global variable initialization

Non-constant global variables **can be optionally initialized**:

```cpp
int g_x; // no explicit initializer (zero-initialized by default)
int g_y {}; // zero initialized
int g_z { 1 }; // initialized with value
```

### Constant global variables

Just like local variables, global variables can be constant. As with all constants, **constant global variables must be initialized**.

```cpp
#include <iostream>
 
const int g_x; // error: constant variables must be initialized
constexpr int g_w; // error: constexpr variables must be initialized
 
const int g_y { 1 };  // const global variable g_y, initialized with a value
constexpr int g_z { 2 }; // constexpr global variable g_z, initialized with a value
 
void doSomething()
{
    // global variables can be seen and used everywhere in the file
    std::cout << g_y << '\n';
    std::cout << g_z << '\n';
}
 
int main()
{
    doSomething();
 
    // global variables can be seen and used everywhere in the file
    std::cout << g_y << '\n';
    std::cout << g_z << '\n';
 
    return 0;
}
// g_y and g_z goes out of scope here
```

### summary

```cpp
// Non-constant global variables
int g_x;                 // defines non-initialized global variable (zero initialized by default)
int g_x {};              // defines explicitly zero-initialized global variable
int g_x { 1 };           // defines explicitly initialized global variable
 
// Const global variables
const int g_y;           // error: const variables must be initialized
const int g_y { 2 };     // defines initialized global constant
 
// Constexpr global variables
constexpr int g_y;       // error: constexpr variables must be initialized
constexpr int g_y { 3 }; // defines initialized global const
```

## Variable shadowing (name hiding)

Each block defines its own scope region. So what happens when we have a variable inside a nested block that has the same name as a variable in an outer block? When this happens, the nested variable “hides” the outer variable in areas where they are both in scope. This is called **name hiding** or **shadowing**.

### Shadowing of local variables

```cpp
#include <iostream>
 
int main()
{ // outer block
    int apples { 5 }; // here's the outer block apples
 
    { // nested block
        // apples refers to outer block apples here
        std::cout << apples << '\n'; // print value of outer block apples
 
        int apples{ 0 }; // define apples in the scope of the nested block
 
        // apples now refers to the nested block apples
        // the outer block apples is temporarily hidden
 
        apples = 10; // this assigns value 10 to nested block apples, not outer block apples
 
        std::cout << apples << '\n'; // print value of nested block apples
    } // nested block apples destroyed
 
 
    std::cout << apples << '\n'; // prints value of outer block apples
 
    return 0;
} // outer block apples destroyed

// OUTPUT 
// 5
// 10
// 5
```

### Shadowing of global variables

```cpp
#include <iostream>
int value { 5 }; // global variable
 
void foo()
{
    std::cout << "global variable value: " << value << '\n'; // value is not shadowed here, so this refers to the global value
}
 
int main()
{
    int value { 7 }; // hides the global variable value until the end of this block
 
    ++value; // increments local value, not global value
 
    std::cout << "local variable value: " << value << '\n';
 
    foo();
 
    return 0;
} // local value is destroyed

// This code prints:

// local variable value: 8
// global variable value: 5
```

However, because global variables are part of the global namespace, we can use the scope operator (::) with no prefix to tell the compiler we mean the global variable instead of the local variable.

```cpp
#include <iostream>
int value { 5 }; // global variable
 
int main()
{
    int value { 7 }; // hides the global variable value
    ++value; // increments local value, not global value
 
    --(::value); // decrements global value, not local value (parenthesis added for readability)
 
    std::cout << "local variable value: " << value << '\n';
    std::cout << "global variable value: " << ::value << '\n';
 
    return 0;
} // local value is destroyed

// This code prints:

// local variable value: 8
// global variable value: 4
```

### Avoid variable shadowing

Shadowing of local variables should generally be avoided, as it can lead to inadvertent errors where the wrong variable is used or modified. Some compilers will issue a warning when a variable is shadowed.

For the same reason that we recommend avoiding shadowing local variables, we recommend avoiding shadowing global variables as well. This is trivially avoidable if all of your global names use a “g_” prefix.

#### Best practice

Avoid variable shadowing.

## Internal linkage

Global variable and functions identifiers can have either **internal linkage** or **external linkage**. 

An identifier with **internal linkage** can be seen and used within a single file, but it is not accessible from other files (that is, it is not exposed to the linker). This means that if two files have identically named identifiers with internal linkage, those identifiers will be treated as independent.

### Global variables with internal linkage

Global variables with internal linkage are sometimes called **internal variables**.

To make a non-constant global variable internal, we use the static keyword.

```cpp
static int g_x; // non-constant globals have external linkage by default, but can be given internal linkage via the static keyword
 
const int g_y { 1 }; // const globals have internal linkage by default
constexpr int g_z { 2 }; // constexpr globals have internal linkage by default
 
int main()
{
    return 0;
}
```

It’s worth noting that internal objects (and functions) that are defined in different files are considered to be independent entities (even if their names and types are identical), so there is no violation of the one-definition rule. Each internal object only has one definition.

### Functions with internal linkage

Because linkage is a property of an identifier (not of a variable), function identifiers have the same linkage property that variable identifiers do. Functions default to external linkage (which we’ll cover in the next lesson), but can be set to internal linkage via the static keyword:

```cpp
// add.cpp:
// Attempts to access it from another file via a function forward declaration will fail
static int add(int x, int y)
{
    return x + y;
}

// main.cpp
#include <iostream>
 
int add(int x, int y); // forward declaration for function add
 
int main()
{
    std::cout << add(3, 4) << '\n';
 
    return 0;
}
```

This program won’t link, because function add is not accessible outside of add.cpp.

```cpp
// Internal global variables definitions:
static int g_x;          // defines non-initialized internal global variable (zero initialized by default)
static int g_x{ 1 };     // defines initialized internal global variable
 
const int g_y { 2 };     // defines initialized internal global const variable
constexpr int g_y { 3 }; // defines initialized internal global constexpr variable
 
// Internal function definitions:
static int foo() {};     // defines internal function
```

## External linkage

An identifier with **external linkage** can be seen and used both from the file in which it is defined, and from other code files (via a forward declaration). In this sense, identifiers with external linkage are truly “global” in that they can be used anywhere in your program!

### Global variables with external linkage

Global variables with external linkage are sometimes called external variables. To make a global variable external (and thus accessible by other files), we can use the extern keyword to do so:

```cpp
int g_x { 2 }; // non-constant globals are external by default
 
extern const int g_y { 3 }; // const globals can be defined as extern, making them external
extern constexpr int g_z { 3 }; // constexpr globals can be defined as extern, making them external (but this is useless, see the note in the next section)
 
int main()
{
    return 0;
}
```

### Variable forward declarations via the extern keyword

To actually use an external global variable that has been defined in another file, you also must place a forward declaration for the global variable in any other files wishing to use the variable. For variables, creating a forward declaration is also done via the extern keyword (with no initialization value).

```cpp
// a.cpp
// global variable definitions
int g_x { 2 }; // non-constant globals have external linkage by default
extern const int g_y { 3 }; // this extern gives g_y external linkage

// main.cpp
#include <iostream>
 
extern int g_x; // this extern is a forward declaration of a variable named g_x that is defined somewhere else
extern const int g_y; // this extern is a forward declaration of a const variable named g_y that is defined somewhere else
 
int main()
{
    std::cout << g_x; // prints 2
 
    return 0;
}
```

### warning

**If you want to define an uninitialized non-const global variable, do not use the extern keyword, otherwise C++ will think you’re trying to make a forward declaration for the variable.**

**Although constexpr variables can be given external linkage via the extern keyword, they can not be forward declared, so there is no value in giving them external linkage.**

Note that function forward declarations don’t need the extern keyword -- the compiler is able to tell whether you’re defining a new function or making a forward declaration based on whether you supply a function body or not. Variables forward declarations do need the extern keyword to help differentiate variables definitions from variable forward declarations (they look otherwise identical):

```cpp
// non-constant 
int g_x; // variable definition (can have initializer if desired)
extern int g_x; // forward declaration (no initializer)
 
// constant
extern const int g_y { 1 }; // variable definition (const requires initializers)
extern const int g_y; // forward declaration (no initializer)

// External global variable definitions:
int g_x;                       // defines non-initialized external global variable (zero initialized by default)
extern const int g_x{ 1 };     // defines initialized const external global variable
extern constexpr int g_x{ 2 }; // defines initialized constexpr external global variable
 
// Forward declarations
extern int g_y;                // forward declaration for non-constant global variable
extern const int g_y;          // forward declaration for const global variable
extern constexpr int g_y;      // not allowed: constexpr variables can't be forward declared
```

## Static local variables

The term _static_ is one of the most confusing terms in the C++ language, in large part because static has different meanings in different contexts.

Using the **static** keyword on a local variable changes its duration from automatic duration to static duration. This means the variable is now created at the start of the program, and destroyed at the end of the program (just like a global variable). As a result, the **static variable** will retain its value even after it goes out of scope!

The easiest way to show the difference between automatic duration and static duration variables is by example.

```cpp
#include <iostream>
 
void incrementAndPrint()
{
    int value{ 1 }; // automatic duration by default
    ++value;
    std::cout << value << '\n';
} // value is destroyed here
 
int main()
{
    incrementAndPrint();
    incrementAndPrint();
    incrementAndPrint();
 
    return 0;
}

// OUTPUT
// 2
// 2
// 2
```

Now consider the static version of this program. The only difference between this and the above program is that we’ve changed the local variable from automatic duration to static duration by using the static keyword.

```cpp
#include <iostream>
 
void incrementAndPrint()
{
    static int s_value{ 1 }; // static duration via static keyword.  This initializer is only executed once.
    ++s_value;
    std::cout << s_value << '\n';
} // s_value is not destroyed here, but becomes inaccessible because it goes out of scope
 
int main()
{
    incrementAndPrint();
    incrementAndPrint();
    incrementAndPrint();
 
    return 0;
}

// OUTPUT
// 2
// 3
// 4
```

Just like we use “g_” to prefix global variables, it’s common to use “s_” to prefix static (static duration) local variables.

> One of the most common uses for static duration local variables is for unique ID generators. Imagine a program where you have many similar objects (e.g. a game where you’re being attacked by many zombies, or a simulation where you’re displaying many triangles). If you notice a defect, it can be near impossible to distinguish which object is having problems. However, if each object is given a unique identifier upon creation, then it can be easier to differentiate the objects for further debugging.

Static variables offer some of the benefit of global variables (they don’t get destroyed until the end of the program) while limiting their visibility to block scope. This makes them safe for use even if you change their values regularly.

> Static local variables should **only** be used if in your entire program and in the foreseeable future of your program, the variable is unique and it wouldn’t make sense to reset the variable.

### Best practice

**Avoid** static local variables unless the variable never needs to be reset. static local variables decrease reusability and make functions harder to reason about.

## Scope, duration, and linkage summary

### Scope summary

An identifier’s scope determines where the identifier can be accessed within the source code.

- Variables with block scope / local scope can only be accessed within the block in which they are declared (including nested blocks). This includes:
	
	- Local variables
	
	- Function parameters
	
	- User-defined type definitions (such as enums and classes) declared inside a block

- Variables and functions with global scope / file scope can be accessed anywhere in the file. This includes:
	
	- Global variables
	
	- Functions
	
	- User-defined type definitions (such as enums and classes) declared inside a namespace or in the global scope

### Duration summary

A variable’s duration determines when it is created and destroyed.

- Variables with automatic duration are created at the point of definition, and destroyed when the block they are part of is exited. This includes:

	- Local variables

	- Function parameters

- Variables with static duration are created when the program begins and destroyed when the program ends. This includes:

	- Global variables

	- Static local variables

- Variables with dynamic duration are created and destroyed by programmer request. This includes:

	- Dynamically allocated variables

### Linkage summary

An identifier’s linkage determines whether multiple declarations of an identifier refer to the same identifier or not.


- An identifier with no linkage means the identifier only refers to itself. This includes:

	- Local variables

	- User-defined type definitions (such as enums and classes) declared inside a block

- An identifier with internal linkage can be accessed anywhere within the file it is declared. This includes:

	- Static global variables (initialized or uninitialized)

	- Static functions

	- Const global variables

	- Functions declared inside an unnamed namespace

	- User-defined type definitions (such as enums and classes) declared inside an unnamed namespace

- An identifier with external linkage can be accessed anywhere within the file it is declared, or other files (via a forward declaration). This includes:

	- Functions

	- Non-const global variables (initialized or uninitialized)

	- Extern const global variables

	- Inline const global variables

	- User-defined type definitions (such as enums and classes) declared inside a namespace or in the global scope

Identifiers with external linkage will generally cause a duplicate definition linker error if the definitions are compiled into more than one .cpp file (due to violating the one-definition rule). There are some exceptions to this rule (for types, templates, and inline functions and variables) -- we’ll cover these further in future lessons when we talk about those topics.

Also note that functions have external linkage by default. They can be made internal by using the static keyword.

### Variable scope, duration, and linkage summary

|Type	|Example	|Scope	|Duration	|Linkage	|Notes|
|:------|:------|:------|:------|:------|:------|
|Local variable	|int x;	|Block	|Automatic	|None|	|
|Static local variable	|static int s_x;	|Block	|Static	|None|	|
|Dynamic variable	|int *x { new int{} };	|Block	|Dynamic	|None|	|	
|Function parameter	|void foo(int x)	|Block	|Automatic	|None|	|	
|External non-constant global variable	|int g_x;	|File	|Static	|External	|Initialized or uninitialized|
|Internal non-constant global variable	|static int g_x;	|File	|Static	|Internal	|Initialized or uninitialized|
|Internal constant global variable	|constexpr int g_x { 1 };	|File	|Static	|Internal	|Must be initialized|
|External constant global variable	|extern constexpr int g_x { 1 };	|File	|Static	|External	|Must be initialized|
|Inline constant global variable	|inline constexpr int g_x { 1 };	|File	|Static	|External	|Must be initialized|
|Internal constant global variable	|const int g_x { 1 };	|File	|Static	|Internal	|Must be initialized|
|External constant global variable	|extern const int g_x { 1 };	|File	|Static	|External	|Must be initialized at definition|
|Inline constant global variable	|inline const int g_x { 1 };	|File	|Static	|External	|Must be initialized|

### Forward declaration summary

|Type	|Example	|Notes|
|:------|:------|:------|
|Function forward declaration	|void foo(int x);	|Prototype only, no function body|
|Non-constant global variable forward declaration	|extern int g_x;	|Must be uninitialized|
|Const global variable forward declaration	|extern const int g_x;	|Must be uninitialized|
|Constexpr global variable forward declaration	|extern constexpr int g_x;	|Not allowed, constexpr cannot be forward declared|

### What the heck is a storage class specifier?

When used as part of an identifier declaration, the _static_ and _extern_ keywords are called **storage class specifiers**. In this context, they set the storage duration and linkage of the identifier.

C++ supports 4 active storage class specifiers:

|Specifier	|Meaning	|Note|
|extern	|static (or thread_local) storage duration and external linkage|	|
|static	|static (or thread_local) storage duration and internal linkage|	|
|thread_local	|thread storage duration	|Introduced in C++11|
|mutable	|object allowed to be modified even if containing class is const|	|
|auto	|automatic storage duration	|Deprecated in C++11|
|register	|automatic storage duration and hint to the compiler to place in a register	|Deprecated in C++17|

The term _storage class specifier_ is typically only used in formal documentation.

## Unnamed and inline namespaces

An **unnamed namespace** (also called an anonymous namespace) is a namespace that is defined without a name, like so:

```cpp
#include <iostream>
 
namespace // unnamed namespace
{
    void doSomething() // can only be accessed in this file
    {
        std::cout << "v1\n";
    }
}
 
int main()
{
    doSomething(); // we can call doSomething() without a namespace prefix
 
    return 0;
}

// OUTPUT
// v1
```

All content declared in an **unnamed namespace** is treated as if it is part of the **parent namespace**. So even though function _doSomething_ is defined in the **unnamed namespace**, the function itself is accessible from the **parent namespace** (which in this case is the global namespace), which is why we can call doSomething from main without any qualifiers.

This might make **unnamed namespaces** seem useless. But the other effect of **unnamed namespaces** is that all identifiers inside an unnamed namespace are treated as if they had **internal linkage**, which means that the content of an unnamed namespace can’t be seen outside of the file in which the unnamed namespace is defined.

For functions, this is effectively the same as defining all functions in the unnamed namespace as static functions. The following program is effectively identical to the one above:

```cpp
#include <iostream>
 
static void doSomething() // can only be accessed in this file
{
    std::cout << "v1\n";
}
 
int main()
{
    doSomething(); // we can call doSomething() without a namespace prefix
 
    return 0;
}
```

**Unnamed namespaces** are typically used when you have a lot of content that you want to ensure stays local to a given file, as it’s easier to cluster such content in an unnamed namespace than individually mark all declarations as static. Unnamed namespaces will also keep user-defined types (something we’ll discuss in a later lesson) local to the file, something for which there is no alternative equivalent mechanism to do.

## Inline Namespace

An **inline namespace** is a namespace that is typically used to version content. Much like an unnamed namespace, anything declared inside an inline namespace is considered part of the parent namespace. However, inline namespaces don’t give everything **internal linkage**.