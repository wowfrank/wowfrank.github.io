---
layout: post
title: "Design Patterns in Python"
date: "2023-04-03 01:09:00 +0800"
description: "Design Patterns in Python" # (optional)
img: "2023-04-03-cover-image-design-patterns-in-python.png" # Add image post (optional)
fig-caption: "" # Add figcaption (optional)
tags: ['python', 'programming language', 'design pattern']
categories: ['python', 'design pattern']
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

    - Abstract Factory: Creates an instance of several families of classes  
        - Provide an interface for creating families of related or dependent objects without specifying their concrete classes.
        - A hierarchy that encapsulates: many possible "platforms", and the construction of a suite of "products".
        - The **_new_** operator considered harmful
    - Builder: Separates object construction from its representation 
        - Separate the construction of a complex object from its representation so that the same construction process can create different representations.
        - Parse a complex representation, create one of several targets.
    - Factory Method: Creates an instance of several derived classes
        - Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.
        - Defining a "virtual" constructor.
        - The **_new_** operator considered harmful.
    - Object Pool: Avoid expensive acquisition and release of resources by recycling objects that are no longer in use
        - Object pooling can offer a significant performance boost; it is most effective in situations where the cost of initializing a class instance is high, the rate of instantiation of a class is high, and the number of instantiations in use at any one time is low.
    - Prototype: A fully initialized instance to be copied or cloned
        - Specify the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype.
        - Co-opt one instance of a class for use as a breeder of all future instances.
        - The **_new_** operator considered harmful.
    - Singleton: A class of which only a single instance can exist
        - Ensure a class has only one instance, and provide a global point of access to it.
        - Encapsulated "just-in-time initialization" or "initialization on first use".

- **Structural patterns** explain how to assemble objects and classes into larger structures, while keeping these structures flexible and efficient.
  
    These design patterns are all about Class and Object composition. Structural class-creation patterns use inheritance to compose interfaces. Structural object-patterns define ways to compose objects to obtain new functionality.

    - Adapter: Match interfaces of different classes
        - Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.
        - Wrap an existing class with a new interface.
        - Impedance match an old component to a new system
    - Bridge: Separates an object’s interface from its implementation
        - Decouple an abstraction from its implementation so that the two can vary independently.
        - Publish interface in an inheritance hierarchy, and bury implementation in its own inheritance hierarchy.
        - Beyond encapsulation, to insulation
    - Composite: A tree structure of simple and composite objects
        - Compose objects into tree structures to represent whole-part hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly.
        - Recursive composition
        - "Directories contain entries, each of which could be a directory."
        - 1-to-many "has a" up the "is a" hierarchy
    - Decorator: Add responsibilities to objects dynamically
        - Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.
        - Client-specified embellishment of a core object by recursively wrapping it.
        - Wrapping a gift, putting it in a box, and wrapping the box.
    - Facade: A single class that represents an entire subsystem
        - Provide a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.
        - Wrap a complicated subsystem with a simpler interface.
    - Flyweight: A fine-grained instance used for efficient sharing
        - Use sharing to support large numbers of fine-grained objects efficiently.
        - The Motif GUI strategy of replacing heavy-weight widgets with light-weight gadgets.
    - Private Class Data: Restricts accessor/mutator access
        - Control write access to class attributes
        - Separate data from methods that use it
        - Encapsulate class data initialization
        - Providing new type of final - final after constructor
    - Proxy: An object representing another object
        - Provide a surrogate or placeholder for another object to control access to it.
        - Use an extra level of indirection to support distributed, controlled, or intelligent access.
        - Add a wrapper and delegation to protect the real component from undue complexity.

- **Behavioral patterns** take care of effective communication and the assignment of responsibilities between objects.

    These design patterns are all about Class's objects communication. Behavioral patterns are those patterns that are most specifically concerned with communication between objects.

    - Chain of responsibility: A way of passing a request between a chain of objects
        - Avoid coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along the chain until an object handles it.
        - Launch-and-leave requests with a single processing pipeline that contains many possible handlers.
        - An object-oriented linked list with recursive traversal.
    - Command: Encapsulate a command request as an object
        - Encapsulate a request as an object, thereby letting you parametrize clients with different requests, queue or log requests, and support undoable operations.
        - Promote "invocation of a method on an object" to full object status
        - An object-oriented callback
    - Interpreter: A way to include language elements in a program
        - Given a language, define a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language.
        - Map a domain to a language, the language to a grammar, and the grammar to a hierarchical object-oriented design.
    - Iterator: Sequentially access the elements of a collection
        - Provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation.
        - The C++ and Java standard library abstraction that makes it possible to decouple collection classes and algorithms.
        - Promote to "full object status" the traversal of a collection.
        - Polymorphic traversal
    - Mediator: Defines simplified communication between classes
        - Define an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it lets you vary their interaction independently.
        - Design an intermediary to decouple many peers.
        - Promote the many-to-many relationships between interacting peers to "full object status".
    - Memento: Capture and restore an object's internal state
        - Without violating encapsulation, capture and externalize an object's internal state so that the object can be returned to this state later.
        - A magic cookie that encapsulates a "check point" capability.
        - Promote undo or rollback to full object status.
    - Null Object: Designed to act as a default value of an object
        - The intent of a Null Object is to encapsulate the absence of an object by providing a substitutable alternative that offers suitable default do nothing behavior. In short, a design where "nothing will come of nothing"
    - Observer: A way of notifying change to a number of classes
        - Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
        - Encapsulate the core (or common or engine) components in a Subject abstraction, and the variable (or optional or user interface) components in an Observer hierarchy.
        - The "View" part of Model-View-Controller.
    - State: Alter an object's behavior when its state changes
        - Allow an object to alter its behavior when its internal state changes. The object will appear to change its class.
        - An object-oriented state machine
        - wrapper + polymorphic wrappee + collaboration
    - Strategy: Encapsulates an algorithm inside a class
        - Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from the clients that use it.
        - Capture the abstraction in an interface, bury implementation details in derived classes.
    - Template method: Defer the exact steps of an algorithm to a subclass
        - Define the skeleton of an algorithm in an operation, deferring some steps to client subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.
        - Base class declares algorithm 'placeholders', and derived classes implement the placeholders.
    - Visitor: Defines a new operation to a class without change
        - Represent an operation to be performed on the elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates.
        - The classic technique for recovering lost type information.
        - Do the right thing based on the type of two objects.
        - Double dispatch

#### 参考

  - [DESIGN PATTERNS in PYTHON](https://refactoring.guru/design-patterns/python)
  - 有具体代码例子 [Design Patterns](https://sourcemaking.com/design_patterns)
