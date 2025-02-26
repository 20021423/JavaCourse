# DAILY TEST 3: MULTIPLE CHOICE QUESTIONS ON SPRING FRAMEWORK

## Multiple Choice Questions

### Question 1
What is the primary purpose of the Spring Framework?
- A) To design user interfaces
- B) To build robust Java applications, especially enterprise applications
- C) To manage databases directly
- D) To write operating systems

**Answer: B) To build robust Java applications, especially enterprise applications**

**Explanation:** The Spring Framework is designed to simplify Java application development, particularly for enterprise-level applications, by providing comprehensive infrastructure support. Options a, c, and d are not the primary focuses of the Spring Framework.

**Knowledge Area:** Overview of Spring Framework

### Question 2
Which core concept does Spring Framework use to manage application objects?
- A) Aspect-Oriented Programming (AOP)
- B) Inversion of Control (IoC) Container
- C) Model-View-Controller (MVC)
- D) Data Access Object (DAO)

**Answer: B) Inversion of Control (IoC) Container**

**Explanation:** Spring's IoC Container is fundamental. It manages application objects (beans), their lifecycle, and dependencies. Options a, c, and d are related concepts or modules within Spring, but IoC Container is the core management principle.

**Knowledge Area:** Inversion of Control (IoC) Container

### Question 3
What does DI stand for in the context of Spring Framework?
- A) Direct Interface
- B) Distributed Instance
- C) Dependency Injection
- D) Dynamic Invocation

**Answer: C) Dependency Injection**

**Explanation:** DI (Dependency Injection) is a key mechanism in Spring for achieving IoC. It allows the container to inject dependencies into objects, rather than objects creating their own dependencies.

**Knowledge Area:** Dependency Injection (DI)

### Question 4
In Spring, what is a 'bean'?
- A) A configuration file
- B) A Java class that represents a database table
- C) An object that is managed by the Spring IoC container
- D) A method in a Spring configuration class

**Answer: C) An object that is managed by the Spring IoC container**

**Explanation:** A bean in Spring is simply an object that the Spring IoC container instantiates, configures, and manages. Options a, b, and d are not accurate definitions of a Spring bean.

**Knowledge Area:** Beans

### Question 5
Which annotation is used to mark a class as a Spring-managed component?
- A) @Bean
- B) @Configuration
- C) @Component
- D) @Autowired

**Answer: C) @Component**

**Explanation:** The `@Component` annotation is a general-purpose annotation to indicate that a class is a Spring-managed component (bean). Options a, b, and d serve different purposes; `@Bean` is for method-level bean declaration, `@Configuration` for configuration classes, and `@Autowired` for dependency injection.

**Knowledge Area:** Annotations (@Component)

### Question 6
What is the role of the Spring IoC container in managing beans?
- A) To compile and run Java code
- B) To manage the lifecycle of beans, including instantiation, configuration, and destruction
- C) To handle web requests and responses
- D) To define database schemas

**Answer: B) To manage the lifecycle of beans, including instantiation, configuration, and destruction**

**Explanation:** The IoC container is responsible for the full lifecycle management of beans. Options a, c, and d describe functionalities outside the primary scope of the IoC container.

**Knowledge Area:** Inversion of Control (IoC) Container

### Question 7
Which type of Dependency Injection involves the container invoking the constructor of a bean?
- A) Setter Injection
- B) Field Injection
- C) Constructor Injection
- D) Interface Injection

**Answer: C) Constructor Injection**

**Explanation:** Constructor Injection is achieved when the Spring container injects dependencies by calling the constructor of the bean class, passing the dependencies as arguments.

**Knowledge Area:** Dependency Injection (DI) - Constructor Injection

### Question 8
Which annotation is a specialization of `@Component` and is typically used for service layer classes?
- A) @Repository
- B) @Controller
- C) @Service
- D) @Configuration

**Answer: C) @Service**

**Explanation:** `@Service` is a specialization of `@Component` and is semantically used to denote classes in the service layer that contain business logic.  `@Repository` is for data access layer, `@Controller` for web layer, and `@Configuration` for configuration classes.

**Knowledge Area:** Annotations (@Service)

### Question 9
Which annotation is used to inject dependencies into a Spring bean?
- A) @Bean
- B) @Component
- C) @Configuration
- D) @Autowired

**Answer: D) @Autowired**

**Explanation:** `@Autowired` is the primary annotation used to instruct Spring to inject dependencies into fields, constructors, or setter methods of a bean.

**Knowledge Area:** Annotations (@Autowired)

### Question 10
Which annotation is used to define a configuration class in Spring?
- A) @Component
- B) @Service
- C) @Configuration
- D) @Repository

**Answer: C) @Configuration**

**Explanation:** `@Configuration` is used to mark a class that provides bean definitions. Methods within classes annotated with `@Configuration` can be annotated with `@Bean` to define specific beans.

**Knowledge Area:** Annotations (@Configuration)

### Question 11
Within a `@Configuration` class, which annotation is used to declare a bean?
- A) @Component
- B) @Autowired
- C) @Bean
- D) @Service

**Answer: C) @Bean**

**Explanation:** Methods annotated with `@Bean` within a `@Configuration` class are used to declare and define beans. The method's return object becomes the bean managed by the Spring container.

**Knowledge Area:** Annotations (@Bean)

### Question 12
What is the default scope of a bean in Spring Framework?
- A) Prototype
- B) Session
- C) Singleton
- D) Request

**Answer: C) Singleton**

**Explanation:** By default, Spring beans are singleton-scoped. This means that for each bean definition, there is only one instance of that bean in the Spring IoC container.

**Knowledge Area:** Bean Scope (Singleton)

### Question 13
Which annotation is used to specify a specific bean to be injected when multiple beans of the same type are available?
- A) @Primary
- B) @Component
- C) @Qualifier
- D) @Autowired

**Answer: C) @Qualifier**

**Explanation:** `@Qualifier` is used in conjunction with `@Autowired` to resolve ambiguity when multiple beans of the same type exist. It allows you to specify which bean should be injected by name.

**Knowledge Area:** Annotations (@Qualifier)

### Question 14
What is the purpose of `@Profile` annotation in Spring?
- A) To define bean scope
- B) To specify bean dependencies
- C) To conditionally enable or disable beans based on the active profile
- D) To define REST endpoints

**Answer: C) To conditionally enable or disable beans based on the active profile**

**Explanation:** `@Profile` allows you to define beans that are only registered when a specific profile or profiles are active. This is useful for different environments like 'dev', 'test', 'prod'.

**Knowledge Area:** Configuration & Profile (@Profile)

### Question 15
Which file types are commonly used for Spring Boot configuration properties?
- A) .xml and .java
- B) .properties and .yml
- C) .html and .css
- D) .js and .json

**Answer: B) .properties and .yml**

**Explanation:** Spring Boot supports both `.properties` and `.yml` (or `.yaml`) files for external configuration. These files are used to configure application properties like server ports, database settings, etc.

**Knowledge Area:** Configuration & Profile (application.properties, application.yml)

### Question 16
If you have two beans implementing the same interface, and you want one to be injected by default when using `@Autowired`, which annotation would you use on the preferred bean?
- A) @Qualifier
- B) @Primary
- C) @Lazy
- D) @Scope

**Answer: B) @Primary**

**Explanation:** `@Primary` is used to indicate that a bean should be given preference when multiple beans are candidates for autowiring. In the absence of a `@Qualifier`, the `@Primary` bean will be chosen.

**Knowledge Area:** Annotations (@Primary)

### Question 17
What is the difference between `@Component` and `@Bean` annotations?
- A) `@Component` is used for configuration classes, and `@Bean` for regular beans.
- B) `@Component` is class-level, used for auto-detection, while `@Bean` is method-level within `@Configuration` classes, used for explicit declaration.
- C) `@Component` is used for singleton beans, and `@Bean` for prototype beans.
- D) There is no difference; they are interchangeable.

**Answer: B) `@Component` is class-level, used for auto-detection, while `@Bean` is method-level within `@Configuration` classes, used for explicit declaration.**

**Explanation:** `@Component` allows Spring to auto-detect and register classes as beans during component scanning. `@Bean` is used within `@Configuration` classes to explicitly declare and configure beans, often for beans you don't control or need more configuration for.

**Knowledge Area:** Annotations (@Component, @Bean)

### Question 18
When would you choose to use constructor injection over setter injection?
- A) When you need to inject optional dependencies.
- B) When you want to allow dependencies to be changed after object creation.
- C) When you want to ensure that dependencies are mandatory and the object is in a valid state upon creation.
- D) Setter injection is generally preferred over constructor injection in all cases.

**Answer: C) When you want to ensure that dependencies are mandatory and the object is in a valid state upon creation.**

**Explanation:** Constructor injection is typically used for mandatory dependencies because it enforces that these dependencies are provided at the time of object creation. This helps in creating immutable and valid objects right from the start. Setter injection is often used for optional dependencies.

**Knowledge Area:** Dependency Injection (DI) - Constructor vs Setter Injection

### Question 19
What happens if Spring cannot find a bean to satisfy an `@Autowired` dependency?
- A) The application will fail to start and throw a `NoSuchBeanDefinitionException`.
- B) Spring will inject a null value for the dependency.
- C) Spring will create a new instance of the required dependency.
- D) It depends on the configuration; it might fail or inject null.

**Answer: A) The application will fail to start and throw a `NoSuchBeanDefinitionException`.**

**Explanation:** By default, if Spring cannot find a bean that matches the dependency required by `@Autowired`, it will throw a `NoSuchBeanDefinitionException` and the application context will fail to load. You can make dependencies optional using `@Autowired(required = false)`.

**Knowledge Area:** Dependency Injection (DI), Exception Handling

### Question 20
What is the purpose of the `CommandLineRunner` interface in Spring Boot?
- A) To handle HTTP requests from the command line.
- B) To run specific code after the Spring application context has been fully initialized and is ready.
- C) To create command-line interfaces for Spring applications.
- D) To manage command-line arguments passed to the application.

**Answer: B) To run specific code after the Spring application context has been fully initialized and is ready.**

**Explanation:** `CommandLineRunner` (and `ApplicationRunner`) interfaces in Spring Boot allow you to run code right after the application context is initialized and ready. This is often used for tasks that need to be executed at application startup, like data initialization or running specific processes.

**Knowledge Area:** Spring Boot Basics (CommandLineRunner)

### Question 21
Which bean scope would you choose if you need a new instance of a bean every time it is requested?
- A) Singleton
- B) Prototype
- C) Request
- D) Session

**Answer: B) Prototype**

**Explanation:** The `prototype` scope creates a new instance of the bean every time it is injected or retrieved from the Spring container. This contrasts with the `singleton` scope where only one instance is shared.

**Knowledge Area:** Bean Scope (Prototype)

### Question 22
What is the role of `SpringApplication.run()` in a Spring Boot application?
- A) To compile the Spring Boot application.
- B) To package the application into a JAR file.
- C) To bootstrap and launch the Spring application context.
- D) To deploy the application to a server.

**Answer: C) To bootstrap and launch the Spring application context.**

**Explanation:** `SpringApplication.run()` is the central method in a Spring Boot application's `main` method. It bootstraps the application, creates and starts the Spring IoC container, and manages the application lifecycle.

**Knowledge Area:** Spring Boot Basics (SpringApplication.run())

### Question 23
How does Spring Boot simplify configuration compared to traditional Spring applications?
- A) By using XML configuration instead of annotations.
- B) By enforcing strict coding conventions, eliminating configuration needs.
- C) By providing auto-configuration, which automatically configures many components based on classpath dependencies and sensible defaults.
- D) Spring Boot does not simplify configuration; it makes it more complex.

**Answer: C) By providing auto-configuration, which automatically configures many components based on classpath dependencies and sensible defaults.**

**Explanation:** Spring Boot's auto-configuration is a key feature that simplifies configuration. It intelligently configures Spring components based on dependencies found in your project and by using reasonable default settings, reducing the need for manual configuration.

**Knowledge Area:** Overview of Spring Framework (Spring Boot)

### Question 24
What is the purpose of `@EnableAutoConfiguration` annotation in Spring Boot?
- A) To disable auto-configuration features.
- B) To explicitly define all configurations manually.
- C) To enable Spring Boot's auto-configuration mechanism, which automatically configures application context.
- D) To configure beans with prototype scope.

**Answer: C) To enable Spring Boot's auto-configuration mechanism, which automatically configures application context.**

**Explanation:** `@EnableAutoConfiguration` is a crucial annotation in Spring Boot that tells Spring Boot to automatically configure your application based on the dependencies you have added. It tries to intelligently configure Spring context.

**Knowledge Area:** Spring Boot Basics (@EnableAutoConfiguration)

### Question 25
Which annotation is used to create RESTful controllers in Spring MVC/Spring Boot?
- A) @Controller
- B) @Component
- C) @RestController
- D) @Service

**Answer: C) @RestController**

**Explanation:** `@RestController` is a specialized version of `@Controller`. It's used for creating RESTful web services. It combines `@Controller` and `@ResponseBody`, indicating that methods return data that should be directly written into the HTTP response body (typically as JSON or XML).

**Knowledge Area:** Annotations (@RestController), Spring MVC

### Question 26
Explain the concept of Inversion of Control (IoC) and how it is implemented in Spring Framework.
- A) IoC is a design pattern where objects control their dependencies, and Spring implements it using annotations.
- B) IoC is a principle where control over object creation and dependencies is inverted to the container, and Spring implements it through its IoC Container using Dependency Injection.
- C) IoC means objects are responsible for creating their own dependencies, and Spring simplifies this process.
- D) IoC is irrelevant to Spring Framework; it’s a general programming concept.

**Answer: B) IoC is a principle where control over object creation and dependencies is inverted to the container, and Spring implements it through its IoC Container using Dependency Injection.**

**Explanation:** IoC inverts the control of object creation and management from the application code to the Spring container. Spring implements IoC through its IoC Container, primarily using Dependency Injection to inject dependencies into beans managed by the container, thus decoupling components and making the application more modular and testable.

**Knowledge Area:** Inversion of Control (IoC) Container, Dependency Injection (DI)

### Question 27
Describe a scenario where you might prefer Field Injection over Constructor or Setter Injection, and also explain why Constructor Injection is generally recommended.
- A) Field Injection is preferred when you have circular dependencies; Constructor Injection is recommended for optional dependencies.
- B) Field Injection is never preferred due to testing difficulties and violation of immutability; Constructor Injection is recommended for mandatory dependencies and promotes immutability.
- C) Field Injection is good for mandatory dependencies, and Constructor Injection for optional ones.
- D) Field Injection and Constructor Injection are interchangeable and preference depends on coding style.

**Answer: B) Field Injection is never preferred due to testing difficulties and violation of immutability; Constructor Injection is recommended for mandatory dependencies and promotes immutability.**

**Explanation:** Field Injection, while convenient, is generally discouraged because it makes testing harder (dependencies are injected even without constructor call, harder to test in isolation) and violates immutability (dependencies can be changed via reflection). Constructor Injection is recommended because it clearly shows mandatory dependencies, promotes immutability, and makes testing easier because dependencies are explicitly passed in the constructor.

**Knowledge Area:** Dependency Injection (DI) - Constructor vs Setter vs Field Injection

### Question 28
Explain the difference between Singleton and Prototype bean scopes and discuss when you would use each.
- A) Singleton creates a new instance every time; Prototype shares a single instance. Use Singleton for stateful beans and Prototype for stateless beans.
- B) Singleton creates a single shared instance per container; Prototype creates a new instance every time requested. Use Singleton for stateless beans and Prototype for stateful beans or when isolation is needed.
- C) Singleton and Prototype are the same; scope is determined by the class definition, not scope annotation.
- D) Singleton is for web applications, and Prototype for standalone applications.

**Answer: B) Singleton creates a single shared instance per container; Prototype creates a new instance every time requested. Use Singleton for stateless beans and Prototype for stateful beans or when isolation is needed.**

**Explanation:** Singleton scope is for creating a single instance shared across the Spring container, suitable for stateless beans. Prototype scope creates a new bean instance each time a request is made, appropriate for stateful beans or when instances need to be isolated from each other.

**Knowledge Area:** Bean Scope (Singleton, Prototype)

### Question 29
How does Spring handle bean dependencies in terms of instantiation order and dependency resolution?
- A) Spring instantiates beans in alphabetical order and resolves dependencies at runtime.
- B) Spring determines dependency order based on bean definitions, using a dependency graph to ensure dependencies are created before dependents, resolving dependencies at container startup.
- C) Spring always instantiates all singleton beans at startup in no particular order and uses lazy resolution for dependencies.
- D) Spring relies on external configuration files to define the instantiation order and dependency resolution is manual.

**Answer: B) Spring determines dependency order based on bean definitions, using a dependency graph to ensure dependencies are created before dependents, resolving dependencies at container startup.**

**Explanation:** Spring container builds a dependency graph from bean definitions to understand bean dependencies. It then instantiates beans in an order that ensures all dependencies are created before the beans that depend on them. Dependency resolution largely happens at container startup, ensuring proper wiring.

**Knowledge Area:** Inversion of Control (IoC) Container, Dependency Injection (DI), Bean Lifecycle

### Question 30
Discuss the advantages of using annotations for configuration over XML configuration in Spring, and are there any scenarios where XML configuration might still be preferred?
- A) Annotations are verbose and harder to maintain, while XML is concise and type-safe. XML is preferred for all configurations.
- B) Annotations provide type safety and are closer to the code, making configuration more maintainable and readable; XML might be preferred for complex external configurations that need to be changed without recompiling code.
- C) Annotations and XML configurations are functionally the same; annotations are just a syntactic sugar. XML is preferred for large applications.
- D) Annotations are only for simple configurations, and XML is required for advanced features like AOP and transactions.

**Answer: B) Annotations provide type safety and are closer to the code, making configuration more maintainable and readable; XML might be preferred for complex external configurations that need to be changed without recompiling code.**

**Explanation:** Annotations offer type safety, are located directly within the code, making configuration more intuitive and easier to maintain. XML configuration can become verbose and harder to manage for complex applications. However, XML might still be preferred in scenarios where configurations are very complex and need to be changed frequently without recompiling the application, as XML can be modified externally. For most modern Spring applications, especially with Spring Boot, annotations are the dominant and recommended approach.

**Knowledge Area:** Annotations, Configuration, Spring Best Practices