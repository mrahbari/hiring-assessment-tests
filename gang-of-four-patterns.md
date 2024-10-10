The **Gang of Four patterns** refer to a set of design patterns introduced in the book *"Design Patterns: Elements of Reusable Object-Oriented Software"* by four authors: Erich Gamma, Richard Helm, Ralph Johnson, and John Vlissides (often referred to as the "Gang of Four" or GoF). These patterns are aimed at solving common software design problems and promoting reusable, scalable, and maintainable object-oriented code.

The GoF cataloged 23 design patterns, grouped into three categories:

1. **Creational Patterns**: Deal with object creation mechanisms, aiming to create objects in a manner that suits the situation best. These include:
   - **Singleton**: Ensures a class has only one instance and provides a global point of access to it.
   - **Factory Method**: Defines an interface for creating an object but lets subclasses alter the type of objects that will be created.
   - **Abstract Factory**: Provides an interface for creating families of related or dependent objects without specifying their concrete classes.
   - **Builder**: Separates the construction of a complex object from its representation.
   - **Prototype**: Allows for the creation of new objects by copying an existing object, known as the prototype.

2. **Structural Patterns**: Focus on how objects are composed and relate to each other, allowing objects to work together efficiently. These include:
   - **Adapter**: Converts the interface of a class into another interface that clients expect, allowing incompatible classes to work together.
   - **Decorator**: Adds additional responsibilities to an object dynamically, providing a flexible alternative to subclassing for extending functionality.
   - **Facade**: Provides a simplified interface to a complex subsystem.
   - **Composite**: Composes objects into tree structures to represent part-whole hierarchies, allowing clients to treat individual objects and compositions of objects uniformly.

3. **Behavioral Patterns**: Deal with object collaboration and the delegation of responsibilities. These include:
   - **Observer**: Defines a dependency between objects so that when one object changes state, all its dependents are notified.
   - **Strategy**: Enables selecting an algorithm’s behavior at runtime.
   - **Command**: Encapsulates a request as an object, allowing for parameterization of clients with different requests and queuing or logging of requests.
   - **Iterator**: Provides a way to access elements of a collection sequentially without exposing the collection’s underlying representation.
   - **State**: Allows an object to alter its behavior when its internal state changes.

These patterns have become foundational concepts in software engineering and are widely adopted across various programming languages and frameworks. They help improve code organization and flexibility, especially in large or complex systems.

