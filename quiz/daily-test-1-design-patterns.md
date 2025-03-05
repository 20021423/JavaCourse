# Daily Test 1: Design Patterns Multiple Choice Quiz

## Quiz Questions

### Question 1
Which design pattern ensures a class has only one instance and provides a global point of access to it?
- A) Factory Method
- B) Singleton
- C) Builder
- D) Prototype

**Answer: B) Singleton**

**Explanation:** The Singleton pattern restricts the instantiation of a class to a single instance and provides a global point of access to that instance. This is useful when exactly one object is needed to coordinate actions across the system, such as a configuration manager or connection pool.

**Knowledge Area:** Creational Design Patterns

### Question 2
In which category of design patterns does the Factory Method pattern belong?
- A) Structural
- B) Behavioral
- C) Creational
- D) Architectural

**Answer: C) Creational**

**Explanation:** The Factory Method pattern belongs to the Creational category. Creational patterns deal with object creation mechanisms, trying to create objects in a manner suitable to the situation. The Factory Method pattern defines an interface for creating an object but lets subclasses decide which class to instantiate.

**Knowledge Area:** Design Pattern Categories

### Question 3
Which design pattern allows an object to alter its behavior when its internal state changes?
- A) Command
- B) Observer
- C) State
- D) Strategy

**Answer: C) State**

**Explanation:** The State pattern allows an object to change its behavior when its internal state changes, appearing to change its class. This pattern is useful for implementing state-dependent behavior without using large conditional statements.

**Knowledge Area:** Behavioral Design Patterns

### Question 4
Which pattern defines a one-to-many dependency between objects so that when one object changes state, all its dependents are notified and updated automatically?
- A) Observer
- B) Mediator
- C) Chain of Responsibility
- D) Command

**Answer: A) Observer**

**Explanation:** The Observer pattern establishes a one-to-many relationship between a subject object and multiple observer objects. When the subject changes state, all registered observers are notified and updated automatically. This is commonly used in implementing distributed event handling systems.

**Knowledge Area:** Behavioral Design Patterns

### Question 5
Which pattern provides a surrogate or placeholder for another object to control access to it?
- A) Decorator
- B) Adapter
- C) Proxy
- D) Bridge

**Answer: C) Proxy**

**Explanation:** The Proxy pattern provides a surrogate or placeholder for another object to control access to it. Common applications include implementing access control, lazy loading, logging, or monitoring real objects. A proxy can perform additional operations before or after forwarding the request to the real object.

**Knowledge Area:** Structural Design Patterns

### Question 6
Which pattern converts the interface of a class into another interface clients expect?
- A) Bridge
- B) Adapter
- C) Decorator
- D) Facade

**Answer: B) Adapter**

**Explanation:** The Adapter pattern converts the interface of a class into another interface the clients expect. It enables classes to work together that couldn't otherwise because of incompatible interfaces. This is like a real-world adapter that allows devices with different plugs to connect to a power source.

**Knowledge Area:** Structural Design Patterns

### Question 7
Which design pattern applies when you need to define a family of algorithms, encapsulate each one, and make them interchangeable?
- A) Template Method
- B) Command
- C) Strategy
- D) Visitor

**Answer: C) Strategy**

**Explanation:** The Strategy pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. This pattern lets the algorithm vary independently from clients that use it. It's particularly useful when you need to switch between different algorithms or behaviors at runtime.

**Knowledge Area:** Behavioral Design Patterns

### Question 8
Which pattern separates an algorithm from an object structure on which it operates?
- A) Iterator
- B) Command
- C) Visitor
- D) Interpreter

**Answer: C) Visitor**

**Explanation:** The Visitor pattern represents an operation to be performed on elements of an object structure. It lets you define a new operation without changing the classes of the elements on which it operates. This pattern is useful when you have a stable hierarchy of classes but need to define new operations on these classes.

**Knowledge Area:** Behavioral Design Patterns

### Question 9
Which design pattern provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation?
- A) Iterator
- B) Composite
- C) Visitor
- D) Flyweight

**Answer: A) Iterator**

**Explanation:** The Iterator pattern provides a way to access the elements of an aggregate object sequentially without exposing its underlying representation. This pattern is widely used in collection frameworks of programming languages to provide a standard way to traverse through different collection types.

**Knowledge Area:** Behavioral Design Patterns

### Question 10
Which pattern is used to create a simplified interface to a complex subsystem of classes?
- A) Bridge
- B) Facade
- C) Proxy
- D) Composite

**Answer: B) Facade**

**Explanation:** The Facade pattern provides a unified interface to a set of interfaces in a subsystem. It defines a higher-level interface that makes the subsystem easier to use by wrapping a complicated subsystem with a simpler interface. This pattern helps to decouple a client from a complex subsystem.

**Knowledge Area:** Structural Design Patterns

### Question 11
Which pattern allows a client to construct a complex object step by step?
- A) Builder
- B) Prototype
- C) Factory Method
- D) Abstract Factory

**Answer: A) Builder**

**Explanation:** The Builder pattern separates the construction of a complex object from its representation, allowing the same construction process to create different representations. It's especially useful when the algorithm for creating a complex object should be independent of the parts that make up the object and how they're assembled.

**Knowledge Area:** Creational Design Patterns

### Question 12
Which pattern is used when you want to use an existing class, but its interface isn't compatible with the rest of your code?
- A) Bridge
- B) Adapter
- C) Decorator
- D) Composite

**Answer: B) Adapter**

**Explanation:** The Adapter pattern is used to convert the interface of a class into another interface that clients expect. Adapters let classes work together that couldn't otherwise because of incompatible interfaces. This pattern is commonly used when integrating new features with existing code or when working with third-party libraries.

**Knowledge Area:** Structural Design Patterns

### Question 13
Which pattern attaches additional responsibilities to an object dynamically?
- A) Decorator
- B) Proxy
- C) Facade
- D) Flyweight

**Answer: A) Decorator**

**Explanation:** The Decorator pattern allows behavior to be added to an individual object, either statically or dynamically, without affecting the behavior of other objects from the same class. It provides a flexible alternative to subclassing for extending functionality and follows the Open/Closed Principle.

**Knowledge Area:** Structural Design Patterns

### Question 14
Which pattern defines a family of algorithms, making them interchangeable within that family?
- A) Command
- B) Mediator
- C) Observer
- D) Strategy

**Answer: D) Strategy**

**Explanation:** The Strategy pattern defines a family of algorithms, encapsulates each one, and makes them interchangeable. Strategy lets the algorithm vary independently from clients that use it. This pattern is useful for situations where you need to dynamically swap between different implementations of an algorithm.

**Knowledge Area:** Behavioral Design Patterns

### Question 15
Which pattern is used to create complex objects with constituent parts that must be created in the same order or using a specific algorithm?
- A) Factory Method
- B) Builder
- C) Prototype
- D) Abstract Factory

**Answer: B) Builder**

**Explanation:** The Builder pattern is designed to provide a flexible solution to various object creation problems. It separates the construction of a complex object from its representation, allowing the same construction process to create different representations step by step, often with a director that orchestrates the building process.

**Knowledge Area:** Creational Design Patterns

### Question 16
What is the primary benefit of using the Singleton pattern?
- A) It improves system performance
- B) It reduces memory usage
- C) It ensures only one instance of a class exists
- D) It simplifies the creation of complex objects

**Answer: C) It ensures only one instance of a class exists**

**Explanation:** The primary benefit of the Singleton pattern is to ensure that a class has only one instance and to provide a global point of access to it. This is useful for coordinating actions across a system, such as a configuration manager, thread pool, cache, or logging facility where multiple instances would lead to incorrect behavior or resource waste.

**Knowledge Area:** Creational Design Patterns - Singleton

### Question 17
Which pattern allows incompatible interfaces to work together?
- A) Composite
- B) Decorator
- C) Adapter
- D) Bridge

**Answer: C) Adapter**

**Explanation:** The Adapter pattern allows objects with incompatible interfaces to collaborate by converting the interface of one class into an interface expected by the clients. It acts as a bridge between two incompatible interfaces, enabling legacy code to work with new code without modifying the original code.

**Knowledge Area:** Structural Design Patterns

### Question 18
Which pattern is best suited for representing part-whole hierarchies of objects?
- A) Composite
- B) Decorator
- C) Facade
- D) Proxy

**Answer: A) Composite**

**Explanation:** The Composite pattern composes objects into tree structures to represent part-whole hierarchies. It lets clients treat individual objects and compositions of objects uniformly. This is particularly useful when working with graphical user interfaces, file systems, or organizational structures where entities can contain other entities.

**Knowledge Area:** Structural Design Patterns

### Question 19
Which design pattern specifies the kinds of objects to create using a prototypical instance?
- A) Factory Method
- B) Prototype
- C) Builder
- D) Abstract Factory

**Answer: B) Prototype**

**Explanation:** The Prototype pattern creates new objects by copying an existing object, known as the prototype. This pattern is used when the cost of creating a new object is more expensive than copying an existing one, or when the system should be independent of how its products are created, composed, and represented.

**Knowledge Area:** Creational Design Patterns

### Question 20
What pattern encapsulates a request as an object?
- A) Command
- B) Memento
- C) Observer
- D) Strategy

**Answer: A) Command**

**Explanation:** The Command pattern encapsulates a request as an object, thereby allowing for parameterization of clients with different requests, queuing of requests, and logging of the requests. It also allows for the support of undoable operations. This pattern decouples the object that invokes the operation from the object that knows how to perform it.

**Knowledge Area:** Behavioral Design Patterns

### Question 21
Which pattern is used to avoid coupling the sender of a request to its receiver?
- A) Mediator
- B) Command
- C) Observer
- D) Chain of Responsibility

**Answer: D) Chain of Responsibility**

**Explanation:** The Chain of Responsibility pattern avoids coupling the sender of a request to its receiver by giving more than one object a chance to handle the request. It chains the receiving objects and passes the request along the chain until an object handles it. This is useful in scenarios like exception handling or request processing pipelines.

**Knowledge Area:** Behavioral Design Patterns

### Question 22
Which design pattern provides a way to include state-specific behavior within an object's behavior?
- A) Template Method
- B) State
- C) Strategy
- D) Visitor

**Answer: B) State**

**Explanation:** The State pattern allows an object to alter its behavior when its internal state changes. The object will appear to change its class. It encapsulates state-specific behavior in separate state classes and delegates behavior to the current state object, rather than implementing all behaviors in the context object itself.

**Knowledge Area:** Behavioral Design Patterns

### Question 23
In the context of design patterns, what does MVC stand for?
- A) Model-View-Controller
- B) Multiple View Components
- C) Managed Virtual Connection
- D) Master-Validator-Client

**Answer: A) Model-View-Controller**

**Explanation:** MVC stands for Model-View-Controller, which is an architectural pattern commonly used for developing user interfaces. It divides an application into three interconnected parts: the Model (data and business logic), the View (user interface), and the Controller (mediates input and converts it to commands for the model or view).

**Knowledge Area:** Architectural Design Patterns

### Question 24
Which pattern encapsulates how a set of objects interact?
- A) Command
- B) Mediator
- C) Observer
- D) Visitor

**Answer: B) Mediator**

**Explanation:** The Mediator pattern defines an object that encapsulates how a set of objects interact. It promotes loose coupling by keeping objects from referring to each other explicitly, and it centralizes complex interactions between objects. This pattern is often used in user interface development to coordinate communications between different UI components.

**Knowledge Area:** Behavioral Design Patterns

### Question 25
Which pattern is used to minimize memory usage by sharing as much as possible with similar objects?
- A) Flyweight
- B) Proxy
- C) Facade
- D) Composite

**Answer: A) Flyweight**

**Explanation:** The Flyweight pattern is used to minimize memory usage or computational expenses by sharing as much as possible with similar objects. It's particularly useful when a large number of similar objects need to be created, such as characters in a text editor or particles in a simulation. The pattern separates intrinsic (shared) state from extrinsic (context-dependent) state.

**Knowledge Area:** Structural Design Patterns

### Question 26
Which pattern defines an interface for creating objects but lets subclasses decide which class to instantiate?
- A) Abstract Factory
- B) Builder
- C) Factory Method
- D) Prototype

**Answer: C) Factory Method**

**Explanation:** The Factory Method pattern defines an interface for creating an object but lets subclasses decide which class to instantiate. It lets a class defer instantiation to subclasses. This pattern is useful when a class cannot anticipate the type of objects it must create or when a class wants its subclasses to specify the objects it creates.

**Knowledge Area:** Creational Design Patterns

### Question 27
Which pattern captures and externalizes an object's internal state so it can be restored later?
- A) Memento
- B) State
- C) Command
- D) Observer

**Answer: A) Memento**

**Explanation:** The Memento pattern captures and externalizes an object's internal state without violating encapsulation, making it possible to restore the object to this state later. It's commonly used for implementing undo mechanisms, snapshots, or transaction rollbacks in applications where you need to restore previous states of objects.

**Knowledge Area:** Behavioral Design Patterns

### Question 28
Which pattern provides a unified interface to a set of interfaces in a subsystem?
- A) Adapter
- B) Bridge
- C) Facade
- D) Decorator

**Answer: C) Facade**

**Explanation:** The Facade pattern provides a simplified interface to a complex subsystem of classes, making it easier to use. It doesn't encapsulate the subsystem but provides a simplified view of it. This pattern is useful when you want to provide a clean, easy-to-understand interface to a complex system of components.

**Knowledge Area:** Structural Design Patterns

### Question 29
What is the main advantage of using the Abstract Factory pattern?
- A) It provides an interface for creating families of related objects
- B) It ensures a class has only one instance
- C) It simplifies object creation by combining multiple Factory Methods
- D) It creates objects by copying existing objects

**Answer: A) It provides an interface for creating families of related objects**

**Explanation:** The Abstract Factory pattern provides an interface for creating families of related or dependent objects without specifying their concrete classes. This pattern is useful when a system needs to be independent from the way its products are created, composed, and represented, and when the system is configured with one of multiple families of products.

**Knowledge Area:** Creational Design Patterns

### Question 30
Which pattern defines a skeleton of an algorithm in a method, deferring some steps to subclasses?
- A) Strategy
- B) Template Method
- C) Builder
- D) State

**Answer: B) Template Method**

**Explanation:** The Template Method pattern defines the skeleton of an algorithm in a method, deferring some steps to subclasses. It lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure. This pattern is useful when you want to implement the invariant parts of an algorithm once and leave it up to subclasses to implement the behavior that can vary.

**Knowledge Area:** Behavioral Design Patterns
