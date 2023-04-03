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

    - Abstract Factory: Creates an instance of several families of classes  
        - Provide an interface for creating families of related or dependent objects without specifying their concrete classes.
        - A hierarchy that encapsulates: many possible "platforms", and the construction of a suite of "products".
        - The **_new_** operator considered harmful
        
        ```python
        """
        Provide an interface for creating families of related or dependent
        objects without specifying their concrete classes.
        """

        import abc


        class AbstractFactory(metaclass=abc.ABCMeta):
            """
            Declare an interface for operations that create abstract product
            objects.
            """

            @abc.abstractmethod
            def create_product_a(self):
                pass

            @abc.abstractmethod
            def create_product_b(self):
                pass


        class ConcreteFactory1(AbstractFactory):
            """
            Implement the operations to create concrete product objects.
            """

            def create_product_a(self):
                return ConcreteProductA1()

            def create_product_b(self):
                return ConcreteProductB1()


        class ConcreteFactory2(AbstractFactory):
            """
            Implement the operations to create concrete product objects.
            """

            def create_product_a(self):
                return ConcreteProductA2()

            def create_product_b(self):
                return ConcreteProductB2()


        class AbstractProductA(metaclass=abc.ABCMeta):
            """
            Declare an interface for a type of product object.
            """

            @abc.abstractmethod
            def interface_a(self):
                pass


        class ConcreteProductA1(AbstractProductA):
            """
            Define a product object to be created by the corresponding concrete
            factory.
            Implement the AbstractProduct interface.
            """

            def interface_a(self):
                pass


        class ConcreteProductA2(AbstractProductA):
            """
            Define a product object to be created by the corresponding concrete
            factory.
            Implement the AbstractProduct interface.
            """

            def interface_a(self):
                pass


        class AbstractProductB(metaclass=abc.ABCMeta):
            """
            Declare an interface for a type of product object.
            """

            @abc.abstractmethod
            def interface_b(self):
                pass


        class ConcreteProductB1(AbstractProductB):
            """
            Define a product object to be created by the corresponding concrete
            factory.
            Implement the AbstractProduct interface.
            """

            def interface_b(self):
                pass


        class ConcreteProductB2(AbstractProductB):
            """
            Define a product object to be created by the corresponding concrete
            factory.
            Implement the AbstractProduct interface.
            """

            def interface_b(self):
                pass


        def main():
            for factory in (ConcreteFactory1(), ConcreteFactory2()):
                product_a = factory.create_product_a()
                product_b = factory.create_product_b()
                product_a.interface_a()
                product_b.interface_b()


        if __name__ == "__main__":
            main()
        ```
    - Builder: Separates object construction from its representation 
        - Separate the construction of a complex object from its representation so that the same construction process can create different representations.
        - Parse a complex representation, create one of several targets.
        
        ```python
        """
        Separate the construction of a complex object from its representation so
        that the same construction process can create different representations.
        """

        import abc


        class Director:
            """
            Construct an object using the Builder interface.
            """

            def __init__(self):
                self._builder = None

            def construct(self, builder):
                self._builder = builder
                self._builder._build_part_a()
                self._builder._build_part_b()
                self._builder._build_part_c()


        class Builder(metaclass=abc.ABCMeta):
            """
            Specify an abstract interface for creating parts of a Product
            object.
            """

            def __init__(self):
                self.product = Product()

            @abc.abstractmethod
            def _build_part_a(self):
                pass

            @abc.abstractmethod
            def _build_part_b(self):
                pass

            @abc.abstractmethod
            def _build_part_c(self):
                pass


        class ConcreteBuilder(Builder):
            """
            Construct and assemble parts of the product by implementing the
            Builder interface.
            Define and keep track of the representation it creates.
            Provide an interface for retrieving the product.
            """

            def _build_part_a(self):
                pass

            def _build_part_b(self):
                pass

            def _build_part_c(self):
                pass


        class Product:
            """
            Represent the complex object under construction.
            """

            pass


        def main():
            concrete_builder = ConcreteBuilder()
            director = Director()
            director.construct(concrete_builder)
            product = concrete_builder.product


        if __name__ == "__main__":
            main()
        ```
    - Factory Method: Creates an instance of several derived classes
        - Define an interface for creating an object, but let subclasses decide which class to instantiate. Factory Method lets a class defer instantiation to subclasses.
        - Defining a "virtual" constructor.
        - The **_new_** operator considered harmful.
        
        ```python
        """
        Define an interface for creating an object, but let subclasses decide
        which class to instantiate. Factory Method lets a class defer
        instantiation to subclasses.
        """

        import abc


        class Creator(metaclass=abc.ABCMeta):
            """
            Declare the factory method, which returns an object of type Product.
            Creator may also define a default implementation of the factory
            method that returns a default ConcreteProduct object.
            Call the factory method to create a Product object.
            """

            def __init__(self):
                self.product = self._factory_method()

            @abc.abstractmethod
            def _factory_method(self):
                pass

            def some_operation(self):
                self.product.interface()


        class ConcreteCreator1(Creator):
            """
            Override the factory method to return an instance of a
            ConcreteProduct1.
            """

            def _factory_method(self):
                return ConcreteProduct1()


        class ConcreteCreator2(Creator):
            """
            Override the factory method to return an instance of a
            ConcreteProduct2.
            """

            def _factory_method(self):
                return ConcreteProduct2()


        class Product(metaclass=abc.ABCMeta):
            """
            Define the interface of objects the factory method creates.
            """

            @abc.abstractmethod
            def interface(self):
                pass


        class ConcreteProduct1(Product):
            """
            Implement the Product interface.
            """

            def interface(self):
                pass


        class ConcreteProduct2(Product):
            """
            Implement the Product interface.
            """

            def interface(self):
                pass


        def main():
            concrete_creator = ConcreteCreator1()
            concrete_creator.product.interface()
            concrete_creator.some_operation()


        if __name__ == "__main__":
            main()
        ```
    - Object Pool: Avoid expensive acquisition and release of resources by recycling objects that are no longer in use
        - Object pooling can offer a significant performance boost; it is most effective in situations where the cost of initializing a class instance is high, the rate of instantiation of a class is high, and the number of instantiations in use at any one time is low.
        
        ```python
        """
        Offer a significant performance boost; it is most effective in
        situations where the cost of initializing a class instance is high, the
        rate of instantiation of a class is high, and the number of
        instantiations in use at any one time is low.
        """


        class ReusablePool:
            """
            Manage Reusable objects for use by Client objects.
            """

            def __init__(self, size):
                self._reusables = [Reusable() for _ in range(size)]

            def acquire(self):
                return self._reusables.pop()

            def release(self, reusable):
                self._reusables.append(reusable)


        class Reusable:
            """
            Collaborate with other objects for a limited amount of time, then
            they are no longer needed for that collaboration.
            """

            pass


        def main():
            reusable_pool = ReusablePool(10)
            reusable = reusable_pool.acquire()
            reusable_pool.release(reusable)


        if __name__ == "__main__":
            main()
        ```
    - Prototype: A fully initialized instance to be copied or cloned
        - Specify the kinds of objects to create using a prototypical instance, and create new objects by copying this prototype.
        - Co-opt one instance of a class for use as a breeder of all future instances.
        - The **_new_** operator considered harmful.
        
        ```python
        """
        Specify the kinds of objects to create using a prototypical instance,
        and create new objects by copying this prototype.
        """

        import copy


        class Prototype:
            """
            Example class to be copied.
            """

            pass


        def main():
            prototype = Prototype()
            prototype_copy = copy.deepcopy(prototype)


        if __name__ == "__main__":
            main()
        ```
    - Singleton: A class of which only a single instance can exist
        - Ensure a class has only one instance, and provide a global point of access to it.
        - Encapsulated "just-in-time initialization" or "initialization on first use".

        ```python
        """
        Ensure a class only has one instance, and provide a global point of
        access to it.
        """


        class Singleton(type):
            """
            Define an Instance operation that lets clients access its unique
            instance.
            """

            def __init__(cls, name, bases, attrs, **kwargs):
                super().__init__(name, bases, attrs)
                cls._instance = None

            def __call__(cls, *args, **kwargs):
                if cls._instance is None:
                    cls._instance = super().__call__(*args, **kwargs)
                return cls._instance


        class MyClass(metaclass=Singleton):
            """
            Example class.
            """

            pass


        def main():
            m1 = MyClass()
            m2 = MyClass()
            assert m1 is m2


        if __name__ == "__main__":
            main()
        ```

- **Structural patterns** explain how to assemble objects and classes into larger structures, while keeping these structures flexible and efficient.
  
    These design patterns are all about Class and Object composition. Structural class-creation patterns use inheritance to compose interfaces. Structural object-patterns define ways to compose objects to obtain new functionality.

    - Adapter: Match interfaces of different classes
        - Convert the interface of a class into another interface clients expect. Adapter lets classes work together that couldn't otherwise because of incompatible interfaces.
        - Wrap an existing class with a new interface.
        - Impedance match an old component to a new system
        ```python
        """
        Convert the interface of a class into another interface clients expect.
        Adapter lets classes work together that couldn't otherwise because of
        incompatible interfaces.
        """

        import abc


        class Target(metaclass=abc.ABCMeta):
            """
            Define the domain-specific interface that Client uses.
            """

            def __init__(self):
                self._adaptee = Adaptee()

            @abc.abstractmethod
            def request(self):
                pass


        class Adapter(Target):
            """
            Adapt the interface of Adaptee to the Target interface.
            """

            def request(self):
                self._adaptee.specific_request()


        class Adaptee:
            """
            Define an existing interface that needs adapting.
            """

            def specific_request(self):
                pass


        def main():
            adapter = Adapter()
            adapter.request()


        if __name__ == "__main__":
            main()
        ```
    - Bridge: Separates an object’s interface from its implementation
        - Decouple an abstraction from its implementation so that the two can vary independently.
        - Publish interface in an inheritance hierarchy, and bury implementation in its own inheritance hierarchy.
        - Beyond encapsulation, to insulation
        
        ```python
        """
        Decouple an abstraction from its implementation so that the two can vary
        independently.
        """

        import abc


        class Abstraction:
            """
            Define the abstraction's interface.
            Maintain a reference to an object of type Implementor.
            """

            def __init__(self, imp):
                self._imp = imp

            def operation(self):
                self._imp.operation_imp()


        class Implementor(metaclass=abc.ABCMeta):
            """
            Define the interface for implementation classes. This interface
            doesn't have to correspond exactly to Abstraction's interface; in
            fact the two interfaces can be quite different. Typically the
            Implementor interface provides only primitive operations, and
            Abstraction defines higher-level operations based on these
            primitives.
            """

            @abc.abstractmethod
            def operation_imp(self):
                pass


        class ConcreteImplementorA(Implementor):
            """
            Implement the Implementor interface and define its concrete
            implementation.
            """

            def operation_imp(self):
                pass


        class ConcreteImplementorB(Implementor):
            """
            Implement the Implementor interface and define its concrete
            implementation.
            """

            def operation_imp(self):
                pass


        def main():
            concrete_implementor_a = ConcreteImplementorA()
            abstraction = Abstraction(concrete_implementor_a)
            abstraction.operation()


        if __name__ == "__main__":
            main()
        ```
    - Composite: A tree structure of simple and composite objects
        - Compose objects into tree structures to represent whole-part hierarchies. Composite lets clients treat individual objects and compositions of objects uniformly.
        - Recursive composition
        - "Directories contain entries, each of which could be a directory."
        - 1-to-many "has a" up the "is a" hierarchy
        
        ```python
        """
        Compose objects into tree structures to represent part-whole
        hierarchies. Composite lets clients treat individual objects and
        compositions of objects uniformly.
        """

        import abc


        class Component(metaclass=abc.ABCMeta):
            """
            Declare the interface for objects in the composition.
            Implement default behavior for the interface common to all classes,
            as appropriate.
            Declare an interface for accessing and managing its child
            components.
            Define an interface for accessing a component's parent in the
            recursive structure, and implement it if that's appropriate
            (optional).
            """

            @abc.abstractmethod
            def operation(self):
                pass


        class Composite(Component):
            """
            Define behavior for components having children.
            Store child components.
            Implement child-related operations in the Component interface.
            """

            def __init__(self):
                self._children = set()

            def operation(self):
                for child in self._children:
                    child.operation()

            def add(self, component):
                self._children.add(component)

            def remove(self, component):
                self._children.discard(component)


        class Leaf(Component):
            """
            Represent leaf objects in the composition. A leaf has no children.
            Define behavior for primitive objects in the composition.
            """

            def operation(self):
                pass


        def main():
            leaf = Leaf()
            composite = Composite()
            composite.add(leaf)
            composite.operation()


        if __name__ == "__main__":
            main()
        ```
    - Decorator: Add responsibilities to objects dynamically
        - Attach additional responsibilities to an object dynamically. Decorators provide a flexible alternative to subclassing for extending functionality.
        - Client-specified embellishment of a core object by recursively wrapping it.
        - Wrapping a gift, putting it in a box, and wrapping the box.
        
        ```python
        """
        Attach additional responsibilities to an object dynamically. Decorators
        provide a flexible alternative to subclassing for extending
        functionality.
        """

        import abc


        class Component(metaclass=abc.ABCMeta):
            """
            Define the interface for objects that can have responsibilities
            added to them dynamically.
            """

            @abc.abstractmethod
            def operation(self):
                pass


        class Decorator(Component, metaclass=abc.ABCMeta):
            """
            Maintain a reference to a Component object and define an interface
            that conforms to Component's interface.
            """

            def __init__(self, component):
                self._component = component

            @abc.abstractmethod
            def operation(self):
                pass


        class ConcreteDecoratorA(Decorator):
            """
            Add responsibilities to the component.
            """

            def operation(self):
                # ...
                self._component.operation()
                # ...


        class ConcreteDecoratorB(Decorator):
            """
            Add responsibilities to the component.
            """

            def operation(self):
                # ...
                self._component.operation()
                # ...


        class ConcreteComponent(Component):
            """
            Define an object to which additional responsibilities can be
            attached.
            """

            def operation(self):
                pass


        def main():
            concrete_component = ConcreteComponent()
            concrete_decorator_a = ConcreteDecoratorA(concrete_component)
            concrete_decorator_b = ConcreteDecoratorB(concrete_decorator_a)
            concrete_decorator_b.operation()


        if __name__ == "__main__":
            main()
        ```
    - Facade: A single class that represents an entire subsystem
        - Provide a unified interface to a set of interfaces in a subsystem. Facade defines a higher-level interface that makes the subsystem easier to use.
        - Wrap a complicated subsystem with a simpler interface.
        
        ```python
        """
        Provide a unified interface to a set of interfaces in a subsystem.
        Facade defines a higher-level interface that makes the subsystem easier
        to use.
        """


        class Facade:
            """
            Know which subsystem classes are responsible for a request.
            Delegate client requests to appropriate subsystem objects.
            """

            def __init__(self):
                self._subsystem_1 = Subsystem1()
                self._subsystem_2 = Subsystem2()

            def operation(self):
                self._subsystem_1.operation1()
                self._subsystem_1.operation2()
                self._subsystem_2.operation1()
                self._subsystem_2.operation2()


        class Subsystem1:
            """
            Implement subsystem functionality.
            Handle work assigned by the Facade object.
            Have no knowledge of the facade; that is, they keep no references to
            it.
            """

            def operation1(self):
                pass

            def operation2(self):
                pass


        class Subsystem2:
            """
            Implement subsystem functionality.
            Handle work assigned by the Facade object.
            Have no knowledge of the facade; that is, they keep no references to
            it.
            """

            def operation1(self):
                pass

            def operation2(self):
                pass


        def main():
            facade = Facade()
            facade.operation()


        if __name__ == "__main__":
            main()
        ```
    - Flyweight: A fine-grained instance used for efficient sharing
        - Use sharing to support large numbers of fine-grained objects efficiently.
        - The Motif GUI strategy of replacing heavy-weight widgets with light-weight gadgets.
        
        ```python
        """
        Use sharing to support large numbers of fine-grained objects
        efficiently.
        """

        import abc


        class FlyweightFactory:
            """
            Create and manage flyweight objects.
            Ensure that flyweights are shared properly. When a client requests a
            flyweight, the FlyweightFactory object supplies an existing instance
            or creates one, if none exists.
            """

            def __init__(self):
                self._flyweights = {}

            def get_flyweight(self, key):
                try:
                    flyweight = self._flyweights[key]
                except KeyError:
                    flyweight = ConcreteFlyweight()
                    self._flyweights[key] = flyweight
                return flyweight


        class Flyweight(metaclass=abc.ABCMeta):
            """
            Declare an interface through which flyweights can receive and act on
            extrinsic state.
            """

            def __init__(self):
                self.intrinsic_state = None

            @abc.abstractmethod
            def operation(self, extrinsic_state):
                pass


        class ConcreteFlyweight(Flyweight):
            """
            Implement the Flyweight interface and add storage for intrinsic
            state, if any. A ConcreteFlyweight object must be sharable. Any
            state it stores must be intrinsic; that is, it must be independent
            of the ConcreteFlyweight object's context.
            """

            def operation(self, extrinsic_state):
                pass


        def main():
            flyweight_factory = FlyweightFactory()
            concrete_flyweight = flyweight_factory.get_flyweight("key")
            concrete_flyweight.operation(None)


        if __name__ == "__main__":
            main()
        ```
    - Private Class Data: Restricts accessor/mutator access
        - Control write access to class attributes
        - Separate data from methods that use it
        - Encapsulate class data initialization
        - Providing new type of final - final after constructor
        
        ```python
        """
        Control write access to class attributes.
        Separate data from methods that use it.
        Encapsulate class data initialization.
        """


        class DataClass:
            """
            Hide all the attributes.
            """

            def __init__(self):
                self.value = None

            def __get__(self, instance, owner):
                return self.value

            def __set__(self, instance, value):
                if self.value is None:
                    self.value = value


        class MainClass:
            """
            Initialize data class through the data class's constructor.
            """

            attribute = DataClass()

            def __init__(self, value):
                self.attribute = value


        def main():
            m = MainClass(True)
            m.attribute = False


        if __name__ == "__main__":
            main()
        ```
    - Proxy: An object representing another object
        - Provide a surrogate or placeholder for another object to control access to it.
        - Use an extra level of indirection to support distributed, controlled, or intelligent access.
        - Add a wrapper and delegation to protect the real component from undue complexity.
        
        ```python
        """
        Provide a surrogate or placeholder for another object to control access
        to it or add other responsibilities.
        """

        import abc


        class Subject(metaclass=abc.ABCMeta):
            """
            Define the common interface for RealSubject and Proxy so that a
            Proxy can be used anywhere a RealSubject is expected.
            """

            @abc.abstractmethod
            def request(self):
                pass


        class Proxy(Subject):
            """
            Maintain a reference that lets the proxy access the real subject.
            Provide an interface identical to Subject's.
            """

            def __init__(self, real_subject):
                self._real_subject = real_subject

            def request(self):
                # ...
                self._real_subject.request()
                # ...


        class RealSubject(Subject):
            """
            Define the real object that the proxy represents.
            """

            def request(self):
                pass


        def main():
            real_subject = RealSubject()
            proxy = Proxy(real_subject)
            proxy.request()


        if __name__ == "__main__":
            main()
        ```

- **Behavioral patterns** take care of effective communication and the assignment of responsibilities between objects.

    These design patterns are all about Class's objects communication. Behavioral patterns are those patterns that are most specifically concerned with communication between objects.

    - Chain of responsibility: A way of passing a request between a chain of objects
        - Avoid coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. Chain the receiving objects and pass the request along the chain until an object handles it.
        - Launch-and-leave requests with a single processing pipeline that contains many possible handlers.
        - An object-oriented linked list with recursive traversal.
        
        ```python
        """
        Avoid coupling the sender of a request to its receiver by giving
        more than one object a chance to handle the request. Chain the receiving
        objects and pass the request along the chain until an object handles it.
        """

        import abc


        class Handler(metaclass=abc.ABCMeta):
            """
            Define an interface for handling requests.
            Implement the successor link.
            """

            def __init__(self, successor=None):
                self._successor = successor

            @abc.abstractmethod
            def handle_request(self):
                pass


        class ConcreteHandler1(Handler):
            """
            Handle request, otherwise forward it to the successor.
            """

            def handle_request(self):
                if True:  # if can_handle:
                    pass
                elif self._successor is not None:
                    self._successor.handle_request()


        class ConcreteHandler2(Handler):
            """
            Handle request, otherwise forward it to the successor.
            """

            def handle_request(self):
                if False:  # if can_handle:
                    pass
                elif self._successor is not None:
                    self._successor.handle_request()


        def main():
            concrete_handler_1 = ConcreteHandler1()
            concrete_handler_2 = ConcreteHandler2(concrete_handler_1)
            concrete_handler_2.handle_request()


        if __name__ == "__main__":
            main()
        ```
    - Command: Encapsulate a command request as an object
        - Encapsulate a request as an object, thereby letting you parametrize clients with different requests, queue or log requests, and support undoable operations.
        - Promote "invocation of a method on an object" to full object status
        - An object-oriented callback
        
        ```python
        """
        Encapsulate a request as an object, thereby letting you parameterize
        clients with different requests, queue or log requests, and support
        undoable operations.
        """

        import abc


        class Invoker:
            """
            Ask the command to carry out the request.
            """

            def __init__(self):
                self._commands = []

            def store_command(self, command):
                self._commands.append(command)

            def execute_commands(self):
                for command in self._commands:
                    command.execute()


        class Command(metaclass=abc.ABCMeta):
            """
            Declare an interface for executing an operation.
            """

            def __init__(self, receiver):
                self._receiver = receiver

            @abc.abstractmethod
            def execute(self):
                pass


        class ConcreteCommand(Command):
            """
            Define a binding between a Receiver object and an action.
            Implement Execute by invoking the corresponding operation(s) on
            Receiver.
            """

            def execute(self):
                self._receiver.action()


        class Receiver:
            """
            Know how to perform the operations associated with carrying out a
            request. Any class may serve as a Receiver.
            """

            def action(self):
                pass


        def main():
            receiver = Receiver()
            concrete_command = ConcreteCommand(receiver)
            invoker = Invoker()
            invoker.store_command(concrete_command)
            invoker.execute_commands()


        if __name__ == "__main__":
            main()
        ```
    - Interpreter: A way to include language elements in a program
        - Given a language, define a representation for its grammar along with an interpreter that uses the representation to interpret sentences in the language.
        - Map a domain to a language, the language to a grammar, and the grammar to a hierarchical object-oriented design.
        
        ```python
        """
        Define a represention for a grammar of the given language along with an
        interpreter that uses the representation to interpret sentences in the
        language.
        """

        import abc


        class AbstractExpression(metaclass=abc.ABCMeta):
            """
            Declare an abstract Interpret operation that is common to all nodes
            in the abstract syntax tree.
            """

            @abc.abstractmethod
            def interpret(self):
                pass


        class NonterminalExpression(AbstractExpression):
            """
            Implement an Interpret operation for nonterminal symbols in the grammar.
            """

            def __init__(self, expression):
                self._expression = expression

            def interpret(self):
                self._expression.interpret()


        class TerminalExpression(AbstractExpression):
            """
            Implement an Interpret operation associated with terminal symbols in
            the grammar.
            """

            def interpret(self):
                pass


        def main():
            abstract_syntax_tree = NonterminalExpression(TerminalExpression())
            abstract_syntax_tree.interpret()


        if __name__ == "__main__":
            main()
        ```
    - Iterator: Sequentially access the elements of a collection
        - Provide a way to access the elements of an aggregate object sequentially without exposing its underlying representation.
        - The C++ and Java standard library abstraction that makes it possible to decouple collection classes and algorithms.
        - Promote to "full object status" the traversal of a collection.
        - Polymorphic traversal
        
        ```python
        """
        Provide a way to access the elements of an aggregate objects equentially
        without exposing its underlying representation.
        """

        import collections.abc


        class ConcreteAggregate(collections.abc.Iterable):
            """
            Implement the Iterator creation interface to return an instance of
            the proper ConcreteIterator.
            """

            def __init__(self):
                self._data = None

            def __iter__(self):
                return ConcreteIterator(self)


        class ConcreteIterator(collections.abc.Iterator):
            """
            Implement the Iterator interface.
            """

            def __init__(self, concrete_aggregate):
                self._concrete_aggregate = concrete_aggregate

            def __next__(self):
                if True:  # if no_elements_to_traverse:
                    raise StopIteration
                return None  # return element


        def main():
            concrete_aggregate = ConcreteAggregate()
            for _ in concrete_aggregate:
                pass


        if __name__ == "__main__":
            main()
        ```
    - Mediator: Defines simplified communication between classes
        - Define an object that encapsulates how a set of objects interact. Mediator promotes loose coupling by keeping objects from referring to each other explicitly, and it lets you vary their interaction independently.
        - Design an intermediary to decouple many peers.
        - Promote the many-to-many relationships between interacting peers to "full object status".
        
        ```python
        """
        Define an object that encapsulates how a set of objects interact.
        Mediator promotes loose coupling by keeping objects from referring to
        each other explicitly, and it lets you vary their interaction
        independently.
        """


        class Mediator:
            """
            Implement cooperative behavior by coordinating Colleague objects.
            Know and maintains its colleagues.
            """

            def __init__(self):
                self._colleague_1 = Colleague1(self)
                self._colleague_2 = Colleague2(self)


        class Colleague1:
            """
            Know its Mediator object.
            Communicate with its mediator whenever it would have otherwise
            communicated with another colleague.
            """

            def __init__(self, mediator):
                self._mediator = mediator


        class Colleague2:
            """
            Know its Mediator object.
            Communicate with its mediator whenever it would have otherwise
            communicated with another colleague.
            """

            def __init__(self, mediator):
                self._mediator = mediator


        def main():
            mediator = Mediator()


        if __name__ == "__main__":
            main()
        ```
    - Memento: Capture and restore an object's internal state
        - Without violating encapsulation, capture and externalize an object's internal state so that the object can be returned to this state later.
        - A magic cookie that encapsulates a "check point" capability.
        - Promote undo or rollback to full object status.
        
        ```python
        """
        Capture and externalize an object's internal state so that the object
        can be restored to this state later, without violating encapsulation.
        """

        import pickle


        class Originator:
            """
            Create a memento containing a snapshot of its current internal
            state.
            Use the memento to restore its internal state.
            """

            def __init__(self):
                self._state = None

            def set_memento(self, memento):
                previous_state = pickle.loads(memento)
                vars(self).clear()
                vars(self).update(previous_state)

            def create_memento(self):
                return pickle.dumps(vars(self))


        def main():
            originator = Originator()
            memento = originator.create_memento()
            originator._state = True
            originator.set_memento(memento)


        if __name__ == "__main__":
            main()
        ```
    - Null Object: Designed to act as a default value of an object
        - The intent of a Null Object is to encapsulate the absence of an object by providing a substitutable alternative that offers suitable default do nothing behavior. In short, a design where "nothing will come of nothing"
        
        ```python
        """
        Encapsulate the absence of an object by providing a substitutable
        alternative that offers suitable default do nothing behavior.
        """

        import abc


        class AbstractObject(metaclass=abc.ABCMeta):
            """
            Declare the interface for Client's collaborator.
            Implement default behavior for the interface common to all classes,
            as appropriate.
            """

            @abc.abstractmethod
            def request(self):
                pass


        class RealObject(AbstractObject):
            """
            Define a concrete subclass of AbstractObject whose instances provide
            useful behavior that Client expects.
            """

            def request(self):
                pass


        class NullObject(AbstractObject):
            """
            Provide an interface identical to AbstractObject's so that a null
            object can be substituted for a real object.
            Implement its interface to do nothing. What exactly it means to do
            nothing depends on what sort of behavior Client is expecting.
            """

            def request(self):
                pass
        ```
    - Observer: A way of notifying change to a number of classes
        - Define a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically.
        - Encapsulate the core (or common or engine) components in a Subject abstraction, and the variable (or optional or user interface) components in an Observer hierarchy.
        - The "View" part of Model-View-Controller.
        
        ```python
        """
        Define a one-to-many dependency between objects so that when one object
        changes state, all its dependents are notified and updatedautomatically.
        """

        import abc


        class Subject:
            """
            Know its observers. Any number of Observer objects may observe a
            subject.
            Send a notification to its observers when its state changes.
            """

            def __init__(self):
                self._observers = set()
                self._subject_state = None

            def attach(self, observer):
                observer._subject = self
                self._observers.add(observer)

            def detach(self, observer):
                observer._subject = None
                self._observers.discard(observer)

            def _notify(self):
                for observer in self._observers:
                    observer.update(self._subject_state)

            @property
            def subject_state(self):
                return self._subject_state

            @subject_state.setter
            def subject_state(self, arg):
                self._subject_state = arg
                self._notify()


        class Observer(metaclass=abc.ABCMeta):
            """
            Define an updating interface for objects that should be notified of
            changes in a subject.
            """

            def __init__(self):
                self._subject = None
                self._observer_state = None

            @abc.abstractmethod
            def update(self, arg):
                pass


        class ConcreteObserver(Observer):
            """
            Implement the Observer updating interface to keep its state
            consistent with the subject's.
            Store state that should stay consistent with the subject's.
            """

            def update(self, arg):
                self._observer_state = arg
                # ...


        def main():
            subject = Subject()
            concrete_observer = ConcreteObserver()
            subject.attach(concrete_observer)
            subject.subject_state = 123


        if __name__ == "__main__":
            main()
        ```
    - State: Alter an object's behavior when its state changes
        - Allow an object to alter its behavior when its internal state changes. The object will appear to change its class.
        - An object-oriented state machine
        - wrapper + polymorphic wrappee + collaboration
        
        ```python
        """
        Allow an object to alter its behavior when its internal state changes.
        The object will appear to change its class.
        """

        import abc


        class Context:
            """
            Define the interface of interest to clients.
            Maintain an instance of a ConcreteState subclass that defines the
            current state.
            """

            def __init__(self, state):
                self._state = state

            def request(self):
                self._state.handle()


        class State(metaclass=abc.ABCMeta):
            """
            Define an interface for encapsulating the behavior associated with a
            particular state of the Context.
            """

            @abc.abstractmethod
            def handle(self):
                pass


        class ConcreteStateA(State):
            """
            Implement a behavior associated with a state of the Context.
            """

            def handle(self):
                pass


        class ConcreteStateB(State):
            """
            Implement a behavior associated with a state of the Context.
            """

            def handle(self):
                pass


        def main():
            concrete_state_a = ConcreteStateA()
            context = Context(concrete_state_a)
            context.request()


        if __name__ == "__main__":
            main()
        ```
    - Strategy: Encapsulates an algorithm inside a class
        - Define a family of algorithms, encapsulate each one, and make them interchangeable. Strategy lets the algorithm vary independently from the clients that use it.
        - Capture the abstraction in an interface, bury implementation details in derived classes.
        
        ```python
        """
        Define a family of algorithms, encapsulate each one, and make them
        interchangeable. Strategy lets the algorithm vary independently from
        clients that use it.
        """

        import abc


        class Context:
            """
            Define the interface of interest to clients.
            Maintain a reference to a Strategy object.
            """

            def __init__(self, strategy):
                self._strategy = strategy

            def context_interface(self):
                self._strategy.algorithm_interface()


        class Strategy(metaclass=abc.ABCMeta):
            """
            Declare an interface common to all supported algorithms. Context
            uses this interface to call the algorithm defined by a
            ConcreteStrategy.
            """

            @abc.abstractmethod
            def algorithm_interface(self):
                pass


        class ConcreteStrategyA(Strategy):
            """
            Implement the algorithm using the Strategy interface.
            """

            def algorithm_interface(self):
                pass


        class ConcreteStrategyB(Strategy):
            """
            Implement the algorithm using the Strategy interface.
            """

            def algorithm_interface(self):
                pass


        def main():
            concrete_strategy_a = ConcreteStrategyA()
            context = Context(concrete_strategy_a)
            context.context_interface()


        if __name__ == "__main__":
            main()
        ```
    - Template method: Defer the exact steps of an algorithm to a subclass
        - Define the skeleton of an algorithm in an operation, deferring some steps to client subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure.
        - Base class declares algorithm 'placeholders', and derived classes implement the placeholders.
        
        ```python
        """
        Define the skeleton of an algorithm in an operation, deferring some
        steps to subclasses. Template Method lets subclasses redefine certain
        steps of an algorithm without changing the algorithm's structure.
        """

        import abc


        class AbstractClass(metaclass=abc.ABCMeta):
            """
            Define abstract primitive operations that concrete subclasses define
            to implement steps of an algorithm.
            Implement a template method defining the skeleton of an algorithm.
            The template method calls primitive operations as well as operations
            defined in AbstractClass or those of other objects.
            """

            def template_method(self):
                self._primitive_operation_1()
                self._primitive_operation_2()

            @abc.abstractmethod
            def _primitive_operation_1(self):
                pass

            @abc.abstractmethod
            def _primitive_operation_2(self):
                pass


        class ConcreteClass(AbstractClass):
            """
            Implement the primitive operations to carry out
            subclass-specificsteps of the algorithm.
            """

            def _primitive_operation_1(self):
                pass

            def _primitive_operation_2(self):
                pass


        def main():
            concrete_class = ConcreteClass()
            concrete_class.template_method()


        if __name__ == "__main__":
            main()
        ```
    - Visitor: Defines a new operation to a class without change
        - Represent an operation to be performed on the elements of an object structure. Visitor lets you define a new operation without changing the classes of the elements on which it operates.
        - The classic technique for recovering lost type information.
        - Do the right thing based on the type of two objects.
        - Double dispatch
        
        ```python
        """
        Represent an operation to be performed on the elements of an object
        structure. Visitor lets you define a new operation without changing the
        classes of the elements on which it operates.
        """

        import abc


        class Element(metaclass=abc.ABCMeta):
            """
            Define an Accept operation that takes a visitor as an argument.
            """

            @abc.abstractmethod
            def accept(self, visitor):
                pass


        class ConcreteElementA(Element):
            """
            Implement an Accept operation that takes a visitor as an argument.
            """

            def accept(self, visitor):
                visitor.visit_concrete_element_a(self)


        class ConcreteElementB(Element):
            """
            Implement an Accept operation that takes a visitor as an argument.
            """

            def accept(self, visitor):
                visitor.visit_concrete_element_b(self)


        class Visitor(metaclass=abc.ABCMeta):
            """
            Declare a Visit operation for each class of ConcreteElement in the
            object structure. The operation's name and signature identifies the
            class that sends the Visit request to the visitor. That lets the
            visitor determine the concrete class of the element being visited.
            Then the visitor can access the element directly through its
            particular interface.
            """

            @abc.abstractmethod
            def visit_concrete_element_a(self, concrete_element_a):
                pass

            @abc.abstractmethod
            def visit_concrete_element_b(self, concrete_element_b):
                pass


        class ConcreteVisitor1(Visitor):
            """
            Implement each operation declared by Visitor. Each operation
            implements a fragment of the algorithm defined for the corresponding
            class of object in the structure. ConcreteVisitor provides the
            context for the algorithm and stores its local state. This state
            often accumulates results during the traversal of the structure.
            """

            def visit_concrete_element_a(self, concrete_element_a):
                pass

            def visit_concrete_element_b(self, concrete_element_b):
                pass


        class ConcreteVisitor2(Visitor):
            """
            Implement each operation declared by Visitor. Each operation
            implements a fragment of the algorithm defined for the corresponding
            class of object in the structure. ConcreteVisitor provides the
            context for the algorithm and stores its local state. This state
            often accumulates results during the traversal of the structure.
            """

            def visit_concrete_element_a(self, concrete_element_a):
                pass

            def visit_concrete_element_b(self, concrete_element_b):
                pass


        def main():
            concrete_visitor_1 = ConcreteVisitor1()
            concrete_element_a = ConcreteElementA()
            concrete_element_a.accept(concrete_visitor_1)


        if __name__ == "__main__":
            main()
        ```

#### 参考

  - [DESIGN PATTERNS in PYTHON](https://refactoring.guru/design-patterns/python)
  - [Design Patterns](https://sourcemaking.com/design_patterns)
