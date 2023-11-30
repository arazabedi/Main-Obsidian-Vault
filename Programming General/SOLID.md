Concept
___
Single Responsibility Principle (SRP) = A class or module should only have one reason to change

Open-Closed Principle (OCP) - A class should be open for extension but closed for modification

Liskov Substitution Principle (LSP) - A subclass should be substitutable for its superclass

Interface Segregation Principle (ISP) - A client should not be forced to depend on methods it does not use

Dependency Inversion Principle (DIP) - High-level modules should not depend on low-level modules. Instead, both should depend on abstractions.

Programming 
___
SRP - Class/module responsible for 1 single well-defined task

OCP - Add new functionality through subclasses

LSP - A subclass can be used in place of a superclass with no problems

ISP - Interfaces should be small and focussed, and should only contain methods actually needed by the clients

DIP - High level modules should depend on interfaces, and low-level modules should implement those interfaces

Practical Application
___
SRP - Separate data storage and data retrieval into two separate classes

OCP - Use interfaces instead of classes when you know that the class you intended to implement will need changing in the future

LSP - Your subclasses should implement all methods of the superclass

ISP - You should not have classes which implement only part of an interface's methods

DIP - A high-level concept should depend on an interface - never a low-level module/class. For example, `ShoppingCart` should rely on the interface
`PaymentProcessorInterface` - not the `PaymentProcessor` class. This way, the two classes are not tightly coupled, and changes made to `PaymentProcessor` do not necessitate changes to `ShoppingCart`.


