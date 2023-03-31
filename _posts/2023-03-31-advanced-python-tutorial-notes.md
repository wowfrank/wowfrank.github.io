---
layout: post
title: "Advanced Python Tutorial Notes"
date: "2023-03-31 00:09:00 +0800"
description: "Advanced Python Tutorial Notes" # (optional)
img: "2023-03-31-cover-advanced-python-tutorial-notes.jpg" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['python', 'programming language']
categories: ['python', 'programming language']
---

# Rules

1. Python is case-sensitive, which means, for example, Name and name have different meanings
2. Use 4 spaces per indentation level
3. All variables should start with a lowercase letter, for example,  var = 4
4. Functions begin with lowercase
5. Classes begins with a capital letter
6. The first letter of a variable, function, or class must be one of the letters (a-z) or (A-Z). Numbers or special characters such as & and% are not allowed
7. Special characters cannot be used in names
8. There are reserved words, such as and, if, else, break, import, and more, which are not allowed in naming. All reserved words can be found here
9. Limit all lines to a maximum of 79 characters
10. Surround top-level function and class definitions with two blank lines
11. Method definitions inside a class are surrounded by a single blank line
12. Code in the core Python distribution should always use UTF-8, and should not have an encoding declaration
13. Imports should usually be on separate lines
14. Imports are always put at the top of the file, just after any module comments and docstrings, and before module globals and constants.
Imports should be grouped in the following order:
    1. Standard library imports.
    1. Related third party imports.
    1. Local application/library specific imports.
You should put a blank line between each group of imports.
15. Absolute imports are recommended
16. When importing a class from a class-containing module, it’s usually okay to spell this
17. Wildcard imports (from <module> import *) should be avoided
18. Module level “dunders” (i.e. names with two leading and two trailing underscores) such as __all__, __author__, __version__, etc. should be placed after the module docstring but before any import statements except from __future__ imports
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

# Data Types

1. List
    1. allows different data types
    2. allows duplicate  element
    3. 

![编程随想]({{site.baseurl}}/assets/img/2023-03-30/bc-10.jpg)



#### 源自[从阮晓寰到“编程随想”：一个普通公民和“极客”如何成了“国家的敌人”？](https://ngocn2.org/article/2023-03-29-program-think-enemy-of-the-state/) 发布于 2023-03-29