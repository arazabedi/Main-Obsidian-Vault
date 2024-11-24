#Java
## Class Inheritance
___
Classes are used to clarify the concepts of the problem domain in object-oriented programming.

Every class we create adds functionality to the programming language.

This functionality is needed to solve the problems that we encounter.

An essential idea behind OOP is that solutions rise from the interactions between objects which are created from classes.

An object in is an independent unit that has a state. This state can be modified by using the methods that the object provides.

Objects are used in co-operation; each has its own area of responsibility. For example, our user interfaces have thus far made use of Scanner objects.

The Object Class
___
Every Java class extends the class `Object`.

Therefore every Java class has at its disposable the use of the methods inherited from the `Object` class.

To change how these methods are defined, we can override them by defining a new implementation. This frequently occurs when defining new `equals` and `hashCode` methods.

Deriving from multiple classes
___
Classes such as `ArrayList` inherit from numerous classes.

When you examine the API of Java's `ArrayList`, you can see that it has the superclass `AbstractList`, which also has the superclass `AbstractCollection`, which only has the superclass `Object`.

Each class can directly extend only one class. However, a class indirectly inherits all the properties of the classes its ancestors extend.

The `extends` keyword
___
By using the above keyword, you signify to Java that the current class is inheriting from whatever class is written after the keyword.

Here is a superclass of `Engine` - `Part`:

```java
public class Part {

    private String identifier;
    private String manufacturer;
    private String description;

    public Part(String identifier, String manufacturer, String description) {
        this.identifier = identifier;
        this.manufacturer = manufacturer;
        this.description = description;
    }

    public String getIdentifier() {
        return identifier;
    }

    public String getDescription() {
        return description;
    }

    public String getManufacturer() {
        return manufacturer;
    }
}
```

The slow way to write `Engine` is as follows:

```java
public class Engine {

    private String engineType;
    private String identifier;
    private String manufacturer;
    private String description;

    public Engine(String engineType, String identifier, String manufacturer, String description) {
        this.engineType = engineType;
        this.identifier = identifier;
        this.manufacturer = manufacturer;
        this.description = description;
    }

    public String getEngineType() {
        return engineType;
    }

    public String getIdentifier() {
        return identifier;
    }

    public String getDescription() {
        return description;
    }

    public String getManufacturer() {
        return manufacturer;
    }
}
```

Instead, use the `extends` keyword and shorten your code:

```java
public class Engine extends Part {

    private String engineType;

    public Engine(String engineType, String identifier, String manufacturer, String description) {
        super(identifier, manufacturer, description);
        this.engineType = engineType;
    }

    public String getEngineType() {
        return engineType;
    }
}
```

This way you don't need to write methods that already exist in the superclass, and any instance variables that the superclass already contains can be chucked into the `super` keyword within the constructor.

The `super` keyword calls the constructor of the superclass. Any new instance variables will require the standard code including any new methods that may need to be added.

>The `super` call bears some resemblance to the `this` call in a constructor; `this` is used to call a constructor of this class, while `super` is used to call a constructor of the superclass. If a constructor uses the constructor of the superclass by calling `super` in it, the `super` call must be on the first line of the constructor. This is similar to the case with calling `this` (must also be the first line of the constructor).


Access modifiers `private`, `protected`, and `public`
___
`private` - visible only to the internal methods of that specific class (subclasses can directly access it or see it).

`public` - accessible anywhere in the programme

`protected` - accessible within the class and its descendants

Calling a superclass method
___
Use the `super` keyword:

```java
@Override
public String toString() {
    return super.toString() + "\n  And let's add my own message to it!";
}
```

Object type dictates method execution
___
Storing a subclass object reference in a superclass type variable is allowed, but only the methods of the superclass would then be available.

```java
Person ollie = new Student("Ollie", "6381 Hollywood Blvd. Los Angeles 90028");
ollie.credits();        // DOESN'T WORK!
ollie.study();              // DOESN'T WORK!
System.out.println(ollie);   // ollie.toString() WORKS
```

An object has at its disposal the methods that relate to its type.

In Java, the terms "type" and "class" are closely related but have slightly different meanings.

Type: In Java, a type refers to a classification of data that defines the set of operations that can be performed on that data and the possible values it can hold. It represents the nature of the data and determines the operations that can be performed on it. Java supports various types, including primitive types (such as int, boolean, char) and reference types (such as classes, interfaces, arrays).

Class: A class in Java is a blueprint or a template that defines the structure and behavior of objects. It is a user-defined data type that encapsulates data (fields) and behavior (methods) into a single unit. Objects are instances of classes, and they can be created based on the blueprint defined by the class. A class can have fields (variables) to store data and methods to perform operations on that data. In Java, classes are the building blocks of object-oriented programming.

In summary, the key difference between type and class in Java is that type refers to the classification of data, encompassing both primitive types and reference types, while class is a specific kind of reference type that serves as a blueprint for creating objects.

>[!info]
>Regardless of type, the method executed on a variable is always chosen based on the actual type of the object. Objects are polymorphic, which means they can be used via many different variable types. The executed method always related the the actual type of the object.

Polymorphism
___
Essentially, polymorphism in Java refers to two different mechanisms: 

1. Method Overloading - more than 1 method of the same name in a single class
2. Method Overriding - replacing a method previously names in a superclass

This is because polymorphism is the idea that a single action can be performed in different ways.

For example, method overloading: 

```java
class Shapes {
  public void area() {
    System.out.println("Find area ");
  }
public void area(int r) {
    System.out.println("Circle area = "+3.14*r*r);
  }

public void area(double b, double h) {
    System.out.println("Triangle area="+0.5*b*h);
  }
public void area(int l, int b) {
    System.out.println("Rectangle area="+l*b);
  }
}

class Main {
  public static void main(String[] args) {
    Shapes myShape = new Shapes();
    
    myShape.area();
    myShape.area(5);
    myShape.area(6.0,1.2);
    myShape.area(6,2);
    
  }
}
```

Method overriding:

```java
class Vehicle{  
  //defining a method  
  void run(){System.out.println("Vehicle is moving");}  
}  
//Creating a child class  
class Car2 extends Vehicle{  
  //defining the same method as in the parent class  
  void run(){System.out.println("car is running safely");}  
  
  public static void main(String args[]){  
  Car2 obj = new Car2();//creating object  
  obj.run();//calling method  
  }  
}  
```

Polymorphism in Java can be classified into two types:

1. Static/Compile-Time Polymorphism
2. Dynamic/Runtime Polymorphism

This refers to the point at which the polymorphism is resolved.

Compile-time polymorphism refers to method overloading (or operator overloading - a feature not supported by Java).

Runtime polymorphism occurs through method overriding.

When to use inheritance
___
You can use inheritance in pursuit of building a hierarchy of concepts. For example, you may have a class for vehicles, within which you contain attributes (instance variables) and behaviour (instance methods). If you wish to have add attributes and behaviour for specific variants of a concept, you can create new classes that inherit other ones. In this case, you could have a class for cars, planes, vans, or boats. These are all subsets of the superset 'vehicle', and thus contain all the attributes and behaviours of a vehicle, but the specific implementation of the behaviours may vary (method overriding).

>[!warning]
>You should not use inheritance for when an object owns or is composed of other objects.
>
>For example, `Car` should not be extended with `Part` or `Engine`. Instead, these should be instance variables if you need to include them.

>[!tip]
>You should always adhere to the Single Responsibility Principle when using inheritance, as there should be only one reason to change.

Single Responsibility Principle
___
A bit poorly worded - 'reason to change' refers to the idea that every module, class, or function should only need changing if one external thing changes.

For example, if the data that a class subsumes changes, you'll need to change that class to accommodate the new data. However, if the way that the data is retrieved also changes, then you'll also have to change your code. This class has 2 reasons to change. The more reasons the change, the more convoluted your code, and the harder it is to update and maintain.