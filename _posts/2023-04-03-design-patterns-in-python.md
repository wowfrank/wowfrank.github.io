---
layout: post
title: "Design Patterns in Python"
date: "2023-04-03 01:09:00 +0800"
description: "Design Patterns in Python" # (optional)
img: "2023-04-03-cover-image-design-patterns-in-python.png" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['python', 'programming language', 'design pattern']
categories: ['python', 'programming language', 'design pattern']
---

# What's a design pattern?

**Design patterns** are typical solutions to commonly occurring problems in software design. They are like pre-made blueprints that you can customize to solve a recurring design problem in your code.

You can’t just find a pattern and copy it into your program, the way you can with off-the-shelf functions or libraries. The pattern is not a specific piece of code, but a general concept for solving a particular problem. You can follow the pattern details and implement a solution that suits the realities of your own program.

**Patterns** are often confused with **algorithms**, because both concepts describe typical solutions to some known problems. While an algorithm always defines a clear set of actions that can achieve some goal, a pattern is a more high-level description of a solution. The code of the same pattern applied to two different programs may be different.

An analogy to an **algorithm** is a cooking recipe: both have clear steps to achieve a goal. On the other hand, a **pattern** is more like a blueprint: you can see what the result and its features are, but the exact order of implementation is up to you.

# What does the pattern consist of?

- Intent of the pattern briefly describes both the problem and the solution.
- Motivation further explains the problem and the solution the pattern makes possible.
- Structure of classes shows each part of the pattern and how they are related.
- Code example in one of the popular programming languages makes it easier to grasp the idea behind the pattern.

# Why should I learn patterns?

The truth is that you might manage to work as a programmer for many years without knowing about a single pattern. A lot of people do just that. Even in that case, though, you might be implementing some patterns without even knowing it. So why would you spend time learning them?

- Design patterns are a toolkit of **tried and tested solutions** to common problems in software design. Even if you never encounter these problems, knowing patterns is still useful because it teaches you how to solve all sorts of problems using principles of object-oriented design.
- Design patterns define a common language that you and your teammates can use to communicate more efficiently. You can say, “Oh, just use a Singleton for that,” and everyone will understand the idea behind your suggestion. No need to explain what a singleton is if you know the pattern and its name.

# Classification of patterns

Design patterns differ by their complexity, level of detail and scale of applicability to the entire system being designed. I like the analogy to road construction: you can make an intersection safer by either installing some traffic lights or building an entire multi-level interchange with underground passages for pedestrians.

The most basic and low-level patterns are often called idioms. They usually apply only to a single programming language.

The most universal and high-level patterns are **architectural patterns**. Developers can implement these patterns in virtually any language. Unlike other patterns, they can be used to design the architecture of an entire application.

In addition, all patterns can be categorized by their _intent_, or purpose. This book covers three main groups of patterns:

- **Creational patterns** provide object creation mechanisms that increase flexibility and reuse of existing code.
   
These design patterns are all about class instantiation. This pattern can be further divided into class-creation patterns and object-creational patterns. While class-creation patterns use inheritance effectively in the instantiation process, object-creation patterns use delegation effectively to get the job done.

  - Abstract Factory
  Creates an instance of several families of classes  
    - Provide an interface for creating families of related or dependent objects without specifying their concrete classes.
    - A hierarchy that encapsulates: many possible "platforms", and the construction of a suite of "products".
    - The new operator considered harmful
  - Builder
  Separates object construction from its representation 
    - Separate the construction of a complex object from its representation so that the same construction process can create different representations.
    - Parse a complex representation, create one of several targets.
  - Factory Method
  Creates an instance of several derived classes
  - Object Pool
  Avoid expensive acquisition and release of resources by recycling objects that are no longer in use
  - Prototype
  A fully initialized instance to be copied or cloned
  - Singleton
  A class of which only a single instance can exist


- **Structural patterns** explain how to assemble objects and classes into larger structures, while keeping these structures flexible and efficient.
  
These design patterns are all about Class and Object composition. Structural class-creation patterns use inheritance to compose interfaces. Structural object-patterns define ways to compose objects to obtain new functionality.

    - Adapter
    Match interfaces of different classes
    - Bridge
    Separates an object’s interface from its implementation
    - Composite
    A tree structure of simple and composite objects
    - Decorator
    Add responsibilities to objects dynamically
    - Facade
    A single class that represents an entire subsystem
    - Flyweight
    A fine-grained instance used for efficient sharing
    - Private Class Data
    Restricts accessor/mutator access
    - Proxy
    An object representing another object

- **Behavioral patterns** take care of effective communication and the assignment of responsibilities between objects.

These design patterns are all about Class's objects communication. Behavioral patterns are those patterns that are most specifically concerned with communication between objects.

    - Chain of responsibility
    A way of passing a request between a chain of objects
    - Command
    Encapsulate a command request as an object
    - Interpreter
    A way to include language elements in a program
    - Iterator
    Sequentially access the elements of a collection
    - Mediator
    Defines simplified communication between classes
    - Memento
    Capture and restore an object's internal state
    - Null Object
    Designed to act as a default value of an object
    - Observer
    A way of notifying change to a number of classes
    - State
    Alter an object's behavior when its state changes
    - Strategy
    Encapsulates an algorithm inside a class
    - Template method
    Defer the exact steps of an algorithm to a subclass
    - Visitor
    Defines a new operation to a class without change

#### 参考 [DESIGN PATTERNS in PYTHON](https://refactoring.guru/design-patterns/python)