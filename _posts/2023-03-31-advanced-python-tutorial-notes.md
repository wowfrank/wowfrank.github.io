---
layout: post
title: "Advanced Python Tutorial Notes"
date: "2023-03-31 00:09:00 +0800"
description: "Advanced Python Tutorial Notes" # (optional)
img: "2023-03-31-cover-advanced-python-tutorial-notes.jpg" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['python', 'programming language'， 'python']
categories: ['python']
---

# Rules | Conventions

1. Python is case-sensitive, which means, for example, Name and name have different meanings
2. Use 4 spaces per indentation level
3. All variables should start with a lowercase letter, for example,  var = 4
4. Functions begin with lowercase
5. Classes begins with a capital letter
6. The first letter of a variable, function, or class must be one of the letters (a-z) or (A-Z). Numbers or special characters such as & and % are not allowed
7. Special characters cannot be used in names
8. There are reserved words, such as and, if, else, break, import, and more, which are not allowed in naming
9. Limit all lines to a maximum of 79 characters
10. Surround top-level function and class definitions with two blank lines
11. Method definitions inside a class are surrounded by a single blank line
12. Code in the core Python distribution should always use UTF-8, and should not have an encoding declaration
13. Imports should usually be on separate lines
14. Imports are always put at the top of the file, just after any module comments and docstrings, and before module globals and constants.  
    Imports should be grouped in the following order:

    1. Standard library imports.
    2. Related third party imports.
    3. Local application/library specific imports.  
    
    You should put a blank line between each group of imports.
15. Absolute imports are recommended
16. When importing a class from a class-containing module, it’s usually okay to spell this
17. Wildcard imports (from \<module\> import *) should be avoided
18. Module level “dunders” (i.e. names with two leading and two trailing underscores) such as \_\_all\_\_, \_\_author\_\_, \_\_version\_\_, etc. should be placed after the module docstring but before any import statements except from \_\_future\_\_ imports
19. single-quoted strings and double-quoted strings are the same
20. Avoid extraneous whitespace in the following situations
    1.  Immediately inside parentheses, brackets or braces: 
    ```python
    # Correct: 
    spam(ham[1], {eggs: 2}) 
    # Wrong: 
    spam( ham[ 1 ], { eggs: 2 } )
    ```
    2.  Between a trailing comma and a following close parenthesis: 
    ```python
    # Correct: 
    foo = (0,) 
    # Wrong: 
    bar = (0, )
    ```
    3.  Immediately before a comma, semicolon, or colon: 
    ```python
    # Correct: 
    if x == 4: print(x, y); x, y = y, x 
    # Wrong: 
    if x == 4 : print(x , y) ; x , y = y , x
    ```
    4.  However, in a slice the colon acts like a binary operator, and should have equal amounts on either side (treating it as the operator with the lowest priority)
    5.  Immediately before the open parenthesis that starts the argument list of a function call: 
    ```python
    # Correct: 
    spam(1) 
    # Wrong: 
    spam (1)
    ```
    6.  Immediately before the open parenthesis that starts an indexing or slicing: 
    ```python
    # Correct: 
    dct['key'] = lst[index] 
    # Wrong: 
    dct ['key'] = lst [index]
    ```
    7.  More than one space around an assignment (or other) operator to align it with another:
    ```python
    # Correct:
    x = 1
    y = 2
    long_variable = 3
    # Wrong:
    x             = 1
    y             = 2
    long_variable = 3
    ```
21. Avoid trailing whitespace anywhere
22. Always surround these binary operators with a single space on either side
23. If operators with different priorities are used, consider adding whitespace around the operators with the lowest priority(ies)
    ```python
    # Correct:
    i = i + 1
    submitted += 1
    x = x*2 - 1
    hypot2 = x*x + y*y
    c = (a+b) * (a-b)
    # Wrong:
    i=i+1
    submitted +=1
    x = x * 2 - 1
    hypot2 = x * x + y * y
    c = (a + b) * (a - b)
    ```
24. Function annotations should use the normal rules for colons and always have spaces around the -> arrow if present
    ```python
    # Correct:
    def munge(input: AnyStr): ...
    def munge() -> PosInt: ...
    # Wrong:
    def munge(input:AnyStr): ...
    def munge()->PosInt: ...
    ```
25. Don’t use spaces around the = sign when used to indicate a keyword argument, or when used to indicate a default value for an unannotated function parameter
    ```python
    # Correct:
    def complex(real, imag=0.0):
        return magic(r=real, i=imag)
    # Wrong:
    def complex(real, imag = 0.0):
        return magic(r = real, i = imag)
    
    # Correct:
    def munge(sep: AnyStr = None): ...
    def munge(input: AnyStr, sep: AnyStr = None, limit=1000): ...
    # Wrong:
    def munge(input: AnyStr=None): ...
    def munge(input: AnyStr, limit = 1000): ...
    ```
26. Compound statements (multiple statements on the same line) are generally discouraged
    ```python
    # Correct:
    if foo == 'blah':
        do_blah_thing()
    do_one()
    do_two()
    do_three()

    # Rather not:

    # Wrong:
    if foo == 'blah': do_blah_thing()
    do_one(); do_two(); do_three()
    ```
27. While sometimes it’s okay to put an if/for/while with a small body on the same line, never do this for multi-clause statements. Also avoid folding such long lines
28. Trailing commas are usually optional, except they are mandatory when making a tuple of one element
29. Comments that contradict the code are worse than no comments. Always make a priority of keeping the comments up-to-date when the code changes!
30. Block comments generally apply to some (or all) code that follows them, and are indented to the same level as that code
31. Use inline comments sparingly
32. Write docstrings for all public modules, functions, classes, and methods. Docstrings are not necessary for non-public methods, but you should have a comment that describes what the method does. This comment should appear after the def line
33. Names that are visible to the user as public parts of the API should follow conventions that reflect usage rather than implementation.
34. Modules should have short, all-lowercase names. Underscores can be used in the module name if it improves readability
35. Class names should normally use the CapWords convention.
36. Names of type variables introduced in PEP 484 should normally use CapWords preferring short names
37. Because exceptions should be classes, the class naming convention applies here
38. Function names should be lowercase, with words separated by underscores as necessary to improve readability.
39. Variable names follow the same convention as function names.
40. Always use self for the first argument to instance methods.
41. Always use cls for the first argument to class methods.
42. Use the function naming rules: lowercase with words separated by underscores as necessary to improve readability.
43. Use one leading underscore only for non-public methods and instance variables.
44. To avoid name clashes with subclasses, use two leading underscores to invoke Python’s name mangling rules.
45. Constants are usually defined on a module level and written in all capital letters with underscores separating words.
46. We can make an instance variable private by adding double underscores (\_\_) before its name
47. We can make an instance variable protected by adding single underscores (\_) before its name
48. All objects share class or static variables
49. All variables which are assigned a value in the class declaration are class variables. And variables that are assigned values inside methods are instance variables

# Data Types

## Mutable

1. list
    1. allows different data types
    2. allows duplicate elements
    3. slice -> my_list[start:stop:step]
       1. start is the first element position to include
       2. stop is exclusive, meaning that the element at position stop won’t be included.
       3. step is the step size. more on this later
       4. start, stop, and step are all optional
       5. Negative values can be used too
    4. comprehensions
    ```python
    [ <expression> for item in list if <conditional> ]
    [x for x in range(1,10) if x % 2 == 0]
    ```
    5. ordered
2. dictionaries
   1. comprehensions
   ```python
   {x: x**2 for x in (2, 4, 6)}
   ```
   2. unordered
3. sets
   1. no duplicate elements
   2. unordered
   
## Immutable

1. numbers
2. booleans
3. strings
   1. ordered
4. tuples
   1. can act as the key in a dictionary
   2. allows duplicate elements
   3. ordered
   4. faster

## Collections

Python’s collections module provides a rich set of specialized container data types carefully designed to approach specific programming problems in a Pythonic and efficient way. The module also provides wrapper classes that make it safer to create custom classes that behave similar to the built-in types dict, list, and str.

# Keywords

## Global

1. A global keyword is a keyword that allows a user to modify a variable outside the current scope
2. If a variable is assigned a value anywhere within the function’s body, it’s assumed to be a local unless explicitly declared as global
3. Variables that are only referenced inside a function are implicitly global
4. We use a global keyword to use a global variable inside a function
5. There is no need to use global keywords outside a function
6. To access a global variable inside a function, there is no need to use a global keyword

## lambda

```python
add10 = lambda x: x + 10
print(add10(5))

def add10_func(x):
    return x + 10
```

## map

map(func, seq)

```python
a = [1, 2, 3, 4, 5]
b = map(lambda x: x*2, a)

c = [x*2 for x in a]
```

## filter

filter(func, seq)

```python
a = [1, 2, 3, 4, 5 , 6]
b = map(lambda x: x%2==0, a)

c = [x for x in a if x%2==0]
```

# Class Method VS Static Method

## Class Method

The @classmethod decorator is a built-in function decorator that is an expression that gets evaluated after your function is defined. The result of that evaluation shadows your function definition. A class method receives the class as an implicit first argument, just like an instance method receives the instance 

## Static Method

A static method does not receive an implicit first argument. A static method is also a method that is bound to the class and not the object of the class. This method can’t access or modify the class state. It is present in a class because it makes sense for the method to be present in class.

## The difference between the Class method and the static method

- A class method takes cls as the first parameter while a static method needs no specific parameters.
- A class method can access or modify the class state while a static method can’t access or modify it.
- In general, static methods know nothing about the class state. They are utility-type methods that take some parameters and work upon those parameters. On the other hand class methods must have class as a parameter.
- We use @classmethod decorator in python to create a class method and we use @staticmethod decorator to create a static method in python.

## When to use the class or static method

- We generally use the class method to create factory methods. Factory methods return class objects ( similar to a constructor ) for different use cases.
- We generally use static methods to create utility functions.

## example

```python
# Python program to demonstrate
# use of class method and static method.
from datetime import date
 
 
class Person:
    def __init__(self, name, age):
        self.name = name
        self.age = age
 
    # a class method to create a Person object by birth year.
    @classmethod
    def fromBirthYear(cls, name, year):
        return cls(name, date.today().year - year)
 
    # a static method to check if a Person is adult or not.
    @staticmethod
    def isAdult(age):
        return age > 18
 
 
person1 = Person('mayank', 21)
person2 = Person.fromBirthYear('mayank', 1996)
 
print(person1.age)
print(person2.age)
 
# print the result
print(Person.isAdult(22))
```

