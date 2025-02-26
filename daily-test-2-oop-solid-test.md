# Daily Test 2: OOP and SOLID Principles (30 Questions with Answers)

## Multiple Choice Questions

### Question 1
Which of the following is NOT a fundamental principle of Object-Oriented Programming?
- A) Encapsulation
- B) Inheritance
- C) Normalization
- D) Polymorphism

**Answer: C) Normalization**

**Explanation:** The four fundamental principles of Object-Oriented Programming are Encapsulation, Inheritance, Polymorphism, and Abstraction. Normalization is a concept related to database design, not OOP.

**Knowledge Area:** Object-Oriented Programming - Core Principles

### Question 2
Which SOLID principle states that a class should have only one reason to change?
- A) Single Responsibility Principle
- B) Open/Closed Principle
- C) Liskov Substitution Principle
- D) Interface Segregation Principle

**Answer: A) Single Responsibility Principle**

**Explanation:** The Single Responsibility Principle (SRP) states that a class should have only one reason to change, meaning it should have only one responsibility or job.

**Knowledge Area:** SOLID Principles - Single Responsibility Principle

### Question 3
What does the "O" in SOLID stand for?
- A) Object-Oriented Principle
- B) Open/Closed Principle
- C) Overriding Principle
- D) Operational Principle

**Answer: B) Open/Closed Principle**

**Explanation:** In SOLID, "O" stands for the Open/Closed Principle, which states that software entities should be open for extension but closed for modification.

**Knowledge Area:** SOLID Principles - Overview

### Question 4
Which OOP concept allows a class to hide its internal data and implementation details from the outside world?
- A) Inheritance
- B) Polymorphism
- C) Encapsulation
- D) Abstraction

**Answer: C) Encapsulation**

**Explanation:** Encapsulation is the OOP concept that allows a class to hide its internal data (using private access modifiers) and implementation details from the outside world, providing access only through well-defined interfaces.

**Knowledge Area:** Object-Oriented Programming - Encapsulation

### Question 5
In the context of SOLID principles, what does the Liskov Substitution Principle state?
- A) Classes should have only one responsibility
- B) Subtypes must be substitutable for their base types
- C) Classes should be open for extension but closed for modification
- D) Interfaces should be specific to client needs

**Answer: B) Subtypes must be substitutable for their base types**

**Explanation:** The Liskov Substitution Principle (LSP) states that objects of a superclass should be replaceable with objects of its subclasses without affecting the correctness of the program.

**Knowledge Area:** SOLID Principles - Liskov Substitution Principle

### Question 6
Which of the following is an example of violating the Open/Closed Principle?
- A) A class with multiple responsibilities
- B) A subclass that cannot substitute its parent class
- C) Modifying existing code to add new functionality
- D) Large interfaces that force clients to implement unnecessary methods

**Answer: C) Modifying existing code to add new functionality**

**Explanation:** The Open/Closed Principle states that software entities should be open for extension but closed for modification. Modifying existing code to add new functionality directly violates this principle. Instead, the code should be designed to allow adding new functionality through extension (e.g., inheritance, interfaces).

**Knowledge Area:** SOLID Principles - Open/Closed Principle

### Question 7
What is the primary purpose of abstraction in OOP?
- A) To allow objects to take different forms
- B) To hide implementation details and show only functionality
- C) To inherit properties and behaviors from parent classes
- D) To prevent direct modification of data

**Answer: B) To hide implementation details and show only functionality**

**Explanation:** Abstraction in OOP is about hiding complex implementation details and exposing only the necessary features or functionalities to the outside world. It focuses on what an object does rather than how it does it.

**Knowledge Area:** Object-Oriented Programming - Abstraction

### Question 8
Which of the following describes polymorphism in OOP?
- A) The ability to hide implementation details
- B) The ability to inherit properties from parent classes
- C) The ability to protect data from external access
- D) The ability for objects to take many forms

**Answer: D) The ability for objects to take many forms**

**Explanation:** Polymorphism in OOP allows objects to take different forms or behave differently in different contexts. It's typically implemented through method overriding and method overloading, allowing objects of different classes to be treated as objects of a common superclass.

**Knowledge Area:** Object-Oriented Programming - Polymorphism

### Question 9
The Interface Segregation Principle suggests that:
- A) Classes should have only one responsibility
- B) Interfaces should be specific to client needs
- C) Subtypes must be substitutable for their base types
- D) High-level modules should not depend on low-level modules

**Answer: B) Interfaces should be specific to client needs**

**Explanation:** The Interface Segregation Principle (ISP) states that clients should not be forced to depend on interfaces they do not use. It suggests creating smaller, more specific interfaces rather than large, general-purpose ones.

**Knowledge Area:** SOLID Principles - Interface Segregation Principle

### Question 10
What is meant by the Dependency Inversion Principle?
- A) Classes should have only one responsibility
- B) Software entities should be open for extension but closed for modification
- C) High-level modules should not depend on low-level modules; both should depend on abstractions
- D) Interfaces should be specific to client needs

**Answer: C) High-level modules should not depend on low-level modules; both should depend on abstractions**

**Explanation:** The Dependency Inversion Principle (DIP) states that high-level modules should not depend on low-level modules; both should depend on abstractions. Additionally, abstractions should not depend on details; details should depend on abstractions.

**Knowledge Area:** SOLID Principles - Dependency Inversion Principle

### Question 11
Which of the following is NOT a benefit of encapsulation?
- A) Data hiding
- B) Increased flexibility
- C) Multiple inheritance
- D) Loose coupling

**Answer: C) Multiple inheritance**

**Explanation:** Multiple inheritance is not a benefit of encapsulation but rather a feature of some object-oriented languages (not Java). Encapsulation provides benefits such as data hiding, increased flexibility, maintainability, and loose coupling.

**Knowledge Area:** Object-Oriented Programming - Encapsulation

### Question 12
Which design concept is most closely related to the Single Responsibility Principle?
- A) Coupling
- B) Cohesion
- C) Inheritance
- D) Polymorphism

**Answer: B) Cohesion**

**Explanation:** Cohesion refers to how closely the responsibilities of a single module or class are related to each other. The Single Responsibility Principle aims to increase cohesion by ensuring a class has only one reason to change or one responsibility.

**Knowledge Area:** SOLID Principles - Single Responsibility Principle

### Question 13
In Java, what is the most common way to implement abstraction?
- A) Using private methods
- B) Using interfaces and abstract classes
- C) Using static methods
- D) Using final variables

**Answer: B) Using interfaces and abstract classes**

**Explanation:** In Java, abstraction is commonly implemented using interfaces and abstract classes. These allow developers to define methods without implementation details (abstract methods) that must be implemented by concrete subclasses.

**Knowledge Area:** Object-Oriented Programming - Abstraction in Java

### Question 14
Which of the following scenarios violates the Liskov Substitution Principle?
- A) A subclass that adds new methods
- B) A subclass that overrides methods but maintains the same behavior
- C) A subclass that throws exceptions not thrown by the parent class
- D) A subclass that implements all abstract methods of the parent class

**Answer: C) A subclass that throws exceptions not thrown by the parent class**

**Explanation:** The Liskov Substitution Principle is violated when a subclass changes the expected behavior of the superclass. A subclass that throws exceptions not thrown by the parent class changes the contract and can break code that uses the parent class.

**Knowledge Area:** SOLID Principles - Liskov Substitution Principle

### Question 15
Which of the following is NOT a way to achieve polymorphism in Java?
- A) Method overloading
- B) Method overriding
- C) Using interfaces
- D) Using private constructors

**Answer: D) Using private constructors**

**Explanation:** Private constructors limit object creation but don't contribute to polymorphism. Polymorphism in Java is achieved through method overloading (compile-time polymorphism), method overriding, and using interfaces (runtime polymorphism).

**Knowledge Area:** Object-Oriented Programming - Polymorphism in Java

### Question 16
What problem does the Dependency Inversion Principle primarily address?
- A) Multiple inheritance
- B) Tight coupling
- C) Code duplication
- D) Memory management

**Answer: B) Tight coupling**

**Explanation:** The Dependency Inversion Principle primarily addresses tight coupling between high-level and low-level modules by introducing abstractions. This reduces dependencies and makes the system more modular and easier to maintain.

**Knowledge Area:** SOLID Principles - Dependency Inversion Principle

### Question 17
Which statement about inheritance is true?
- A) Java supports multiple inheritance through classes
- B) Private members of a superclass are accessible in its subclasses
- C) Inheritance is a "has-a" relationship
- D) Inheritance allows for code reuse

**Answer: D) Inheritance allows for code reuse**

**Explanation:** Inheritance enables code reuse by allowing a class to inherit properties and methods from another class. Java doesn't support multiple inheritance through classes (only through interfaces), private members of a superclass are not accessible in its subclasses, and inheritance represents an "is-a" relationship (not "has-a," which is composition).

**Knowledge Area:** Object-Oriented Programming - Inheritance

### Question 18
Which SOLID principle is most concerned with making sure client code doesn't have unwanted dependencies?
- A) Single Responsibility Principle
- B) Open/Closed Principle
- C) Liskov Substitution Principle
- D) Interface Segregation Principle

**Answer: D) Interface Segregation Principle**

**Explanation:** The Interface Segregation Principle is most concerned with ensuring client code doesn't have unwanted dependencies by advocating for smaller, client-specific interfaces instead of large, general-purpose ones.

**Knowledge Area:** SOLID Principles - Interface Segregation Principle

### Question 19
What is the main difference between method overloading and method overriding?
- A) Method overloading occurs in the same class, while method overriding occurs in subclasses
- B) Method overloading changes return types, while method overriding doesn't
- C) Method overloading is a compile-time feature, while method overriding is a runtime feature
- D) Method overloading requires inheritance, while method overriding doesn't

**Answer: C) Method overloading is a compile-time feature, while method overriding is a runtime feature**

**Explanation:** Method overloading is resolved at compile time (also called static binding) and involves multiple methods with the same name but different parameters in the same class. Method overriding is resolved at runtime (dynamic binding) and involves a subclass providing a specific implementation of a method already defined in its superclass.

**Knowledge Area:** Object-Oriented Programming - Polymorphism (Method Overloading vs. Overriding)

### Question 20
Which of the following best demonstrates the Open/Closed Principle?
- A) Using private fields to encapsulate data
- B) Creating an interface and implementing it in multiple classes
- C) Making a class final to prevent inheritance
- D) Using static methods for utility functions

**Answer: B) Creating an interface and implementing it in multiple classes**

**Explanation:** Creating an interface and implementing it in multiple classes demonstrates the Open/Closed Principle because it allows for extending functionality through new implementations without modifying existing code. The system is open for extension (new implementations) but closed for modification.

**Knowledge Area:** SOLID Principles - Open/Closed Principle

### Question 21
What is the relationship between a class and an object?
- A) A class is an instance of an object
- B) An object is an instance of a class
- C) A class and an object are the same thing
- D) An object contains multiple classes

**Answer: B) An object is an instance of a class**

**Explanation:** A class is a blueprint or template that defines the structure and behavior of objects, while an object is an instance of a class created at runtime with specific values for the attributes defined in the class.

**Knowledge Area:** Object-Oriented Programming - Classes and Objects

### Question 22
Which of the following is NOT a characteristic of a well-designed class that follows the Single Responsibility Principle?
- A) It has high cohesion
- B) It has multiple public methods
- C) It has multiple reasons to change
- D) It performs a single logical task

**Answer: C) It has multiple reasons to change**

**Explanation:** A well-designed class that follows the Single Responsibility Principle should have only one reason to change, meaning it has a single, well-defined responsibility. Having multiple reasons to change directly violates this principle.

**Knowledge Area:** SOLID Principles - Single Responsibility Principle

### Question 23
What is composition in OOP?
- A) A relationship where one class inherits from another
- B) A relationship where one class contains an instance of another class
- C) A relationship where one class implements an interface
- D) A relationship where two classes share the same methods

**Answer: B) A relationship where one class contains an instance of another class**

**Explanation:** Composition is a design principle where a class contains an instance of another class as a field or property, representing a "has-a" relationship. This is in contrast to inheritance, which represents an "is-a" relationship.

**Knowledge Area:** Object-Oriented Programming - Composition

### Question 24
What does it mean for a class to be "immutable" in Java?
- A) The class cannot be extended
- B) Instances of the class cannot be modified after creation
- C) The class cannot have constructors
- D) The class cannot implement interfaces

**Answer: B) Instances of the class cannot be modified after creation**

**Explanation:** An immutable class in Java is one whose instances cannot be modified after creation. This is typically achieved by making all fields final, not providing setter methods, ensuring proper encapsulation, and ensuring that mutable composed objects cannot be modified.

**Knowledge Area:** Object-Oriented Programming - Immutability

### Question 25
Which statement best describes the relationship between encapsulation and data hiding?
- A) They are completely unrelated concepts
- B) Encapsulation is a technique that implements data hiding
- C) Data hiding is a technique that implements encapsulation
- D) They are synonyms for the same concept

**Answer: B) Encapsulation is a technique that implements data hiding**

**Explanation:** Encapsulation is a broader concept that includes data hiding as one of its aspects. Encapsulation is the bundling of data and methods that operate on that data, with restrictions on accessing the data directly. Data hiding specifically refers to the practice of making data members private to prevent direct access.

**Knowledge Area:** Object-Oriented Programming - Encapsulation and Data Hiding

### Question 26
Which of the following is NOT a benefit of following the SOLID principles?
- A) Increased maintainability
- B) Improved code reusability
- C) Enhanced performance
- D) Better testability

**Answer: C) Enhanced performance**

**Explanation:** While following SOLID principles leads to better code organization, maintainability, reusability, and testability, it doesn't necessarily lead to enhanced performance. In fact, the additional abstractions might introduce a minor performance overhead, though this is usually negligible compared to the architectural benefits.

**Knowledge Area:** SOLID Principles - Benefits and Trade-offs

### Question 27
What is the primary purpose of an abstract class in Java?
- A) To create objects with default implementations
- B) To provide a base for subclasses with some common functionality
- C) To implement multiple inheritance
- D) To encapsulate data within private methods

**Answer: B) To provide a base for subclasses with some common functionality**

**Explanation:** The primary purpose of an abstract class in Java is to serve as a base class for subclasses to extend, providing some common functionality while requiring subclasses to implement specific methods (abstract methods). It allows for code reuse while enforcing a certain structure.

**Knowledge Area:** Object-Oriented Programming - Abstraction (Abstract Classes)

### Question 28
Which of the following demonstrates proper implementation of the Dependency Inversion Principle?
- A) A high-level module directly instantiates a low-level module
- B) A high-level module accepts an interface in its constructor
- C) A low-level module extends a high-level module
- D) A high-level module uses static methods from a low-level module

**Answer: B) A high-level module accepts an interface in its constructor**

**Explanation:** The Dependency Inversion Principle states that high-level modules shouldn't depend on low-level modules; both should depend on abstractions. A high-level module accepting an interface (abstraction) in its constructor demonstrates proper implementation of this principle, as it depends on the abstraction rather than a concrete implementation.

**Knowledge Area:** SOLID Principles - Dependency Inversion Principle

### Question 29
What is the main disadvantage of using inheritance compared to composition?
- A) Inheritance leads to tighter coupling
- B) Inheritance doesn't allow code reuse
- C) Inheritance is harder to implement
- D) Inheritance consumes more memory

**Answer: A) Inheritance leads to tighter coupling**

**Explanation:** The main disadvantage of inheritance compared to composition is that it leads to tighter coupling between classes. Changes in the parent class can directly affect all subclasses, making the system more rigid and harder to maintain. This is often summarized as "favor composition over inheritance."

**Knowledge Area:** Object-Oriented Programming - Inheritance vs. Composition

### Question 30
Which SOLID principle helps prevent the "God Object" anti-pattern?
- A) Single Responsibility Principle
- B) Open/Closed Principle
- C) Liskov Substitution Principle
- D) Interface Segregation Principle

**Answer: A) Single Responsibility Principle**

**Explanation:** The "God Object" anti-pattern refers to an object that knows too much or does too much, often becoming a monolithic class with numerous responsibilities. The Single Responsibility Principle directly addresses this by advocating that a class should have only one responsibility, thus preventing objects from growing into "God Objects."

**Knowledge Area:** SOLID Principles - Single Responsibility Principle
