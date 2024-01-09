---
layout: post
title: "Decorators in Python"
date: "2024-01-08 01:09:00 +0800"
description: "Decorators in Python" # (optional)
img: "2024-01-08-cover-image-decorators-in-python.png" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['Python']
categories: ['Programming', 'OOP']
---

## What is Decorator

> In Python, a decorator is a design pattern that allows you to modify the functionality of a function by wrapping it in another function.

> The outer function is called the decorator, which takes the original function as an argument and returns a modified version of it.

## Examples

```python
def make_pretty(func):
    # define the inner function 
    def inner():
        # add some additional behavior to decorated function
        print("I got decorated")

        # call original function
        func()
    # return the inner function
    return inner

# define ordinary function
def ordinary():
    print("I am ordinary")
    
# decorate the ordinary function
decorated_func = make_pretty(ordinary)

# call the decorated function
decorated_func()

# output 
# I got decorated
# I am ordinary
```

or simplify it using @symbol

```python
def make_pretty(func):

    def inner():
        print("I got decorated")
        func()
    return inner

@make_pretty
def ordinary():
    print("I am ordinary")

ordinary()  
```

## Decorating Functions with Parameters

```python
def smart_divide(func):
    def inner(a, b):
        print("I am going to divide", a, "and", b)
        if b == 0:
            print("Whoops! cannot divide")
            return

        return func(a, b)
    return inner

@smart_divide
def divide(a, b):
    print(a/b)

divide(2,5)

divide(2,0)
```

## 5 Useful Decorators

1. staticmethod: This decorator is used to define a static method within a class. Static methods don't depend on the instance state and can be called on the class itself. They are often used to group related utility functions that don't need access to instance-specific data.

```python
class SampleClass:
    @staticmethod
    def add(x, y):
        return x + y
```

2. classmethod:Similar to @ staticmethod, the @ classmethod decorator defines a method that is bound to the class and not the instance. Class methods take the class itself as the first argument and can be used for factory methods or alternative constructors.

```python
class Date:
    def __init__(self, day, month, year):
        self.day = day
        self.month = month
        self.year = year
    
    @classmethod
    def from_string(cls, date_string):
        day, month, year = map(int, date_string.split('-'))

        return cls(day, month, year)
```

3. property:The @ property decorator allows you to define a method as a getter for a class attribute. It's a way to make attributes appear read-only, and it can be useful for encapsulating the internal state of an object.

```python
class Circle:
    def __init(self, radius):
        self.radius = radius

    @property
    def diameter(self):
        return self.radius * 2
```

4. staticmethod and property combination:Combining @ staticmethod and @ property, you can create computed properties without the need for explicit getter methods. This reduces code by eliminating the need for separate getter methods.

```python
class Circle:
    def __init(self, radius):
        self.radius = radius

    @staticmethod
    def from_diamether(diameter):
        return Circle(diameter / 2)

    @property
    def diameter(self):
        return self.radius * 2
```

5. wraps:The @ wraps decorator, from the functools module, is used to preserve the metadata of the original function when creating wrapper functions. It's often used when creating decorators to ensure that the decorated function maintains its name, docstring, and other attributes.

```python
from functools import wraps

def my_decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)

    return wrapper
```