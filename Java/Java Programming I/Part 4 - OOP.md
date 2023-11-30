A class specifies what the objects instantiated from it are like.

-   The **object's variables (instance variables)** specify the internal state of the object
-   The **object's methods** specify what the object does

*Classes also have class methods that are related to the class itself...*

## Class vs Instance Methods

In object-oriented programming, both class methods and instance methods are used to define behavior within classes, but they have some 
key differences in how they are accessed and what they operate on:

1.  Definition:
    
    -   Class methods are defined on the class itself. They belong to the class and are shared among all instances of the class.
    -   Instance methods are defined on instances of the class. They operate on the specific instance of the class that calls them.
2.  Access:
    
    -   Class methods are accessed directly on the class itself, typically using the class name followed by the method name (e.g., `ClassName.method_name`).
    -   Instance methods are accessed on individual instances of the class using the dot notation (e.g., `instance.method_name`).
3.  Scope and Context:
    
    -   Class methods are executed in the context of the class itself, which means they have access to class-level variables, other class methods, and can perform actions that are relevant to the entire class.
    -   Instance methods are executed in the context of a specific instance of the class. They have access to instance variables, can interact with the specific state of the instance, and perform actions specific to that instance.
4.  Usage:
    
    -   Class methods are often used for operations that are independent of any specific instance of the class, such as utility methods, factory methods, or methods that perform computations based on class-level data.
    -   Instance methods are used for actions and behaviors that are specific to individual instances of the class. They typically interact with instance variables, encapsulate the state and behavior of each instance, and provide functionality that is unique to each instance.
5.  Self-reference:
    
    -   Within a class method, `self` refers to the class itself.
    -   Within an instance method, `self` refers to the instance of the class that called the method.

It's important to understand these differences and choose the appropriate method type based on the intended functionality and context within your program.



## Introduction to OOP
___
You can define instance variables as such:

```java
public class Person {
    private String name;
    private int age;
}
```

The `private` keyword 'hides' the variables inside the object. This is known as **encapsulation**.

![[Pasted image 20230516165007.png]]

The above class is a blueprint for a Person object. Every Person object has a name and an age.

The 'state' of the variable consists of the values assigned to the instance variables `name` and `age`.

**Defining a Constructor**
___
A constructor is a method that creates an object (an object is the same thing as a class instance).

```java
public class Person {
    private String name;
    private int age;

    public Person(String initialName) {
        this.age = 0;
        this.name = initialName;
    }
}
```

Above we are saying that when you write

```java
Person araz = new Person("Araz")
```

You will create a new instance of the Person class, and set its age instance variable to 0, and its initialName instance variable to "Araz".

*The constructor method is public, and its name is always the same as the class name.*

![[Pasted image 20230516234646.png]]

>[!tip]
>If you don't write a constructor, Java will create a default one that sets primitive instance variables to 0, and all others to null.
>
>Also note that if parameters have been defined for a constructor, you must give parameters to the constructor method when creating a new object, or there will be an error.

**Defining Instance Methods**
___
A method is written inside of the class beneath the constructor.

```java
public class Person {
    private String name;
    private int age;

    public Person(String initialName) {
        this.age = 0;
        this.name = initialName;
    }

    public void printPerson() {
        System.out.println(this.name + ", age " + this.age + " years");
    }
}
```

The above method is `public void` because we don't need it to be hidden, and it doesn't need to return any value. It isn't `static` because it is an instance variable, not a class variable.

![[Pasted image 20230517000131.png]]

**Changing the value of an instance variable**
___
```java
public class Person {
    private String name;
    private int age;

    public Person(String initialName) {
        this.age = 0;
        this.name = initialName;
    }

    public void printPerson() {
        System.out.println(this.name + ", age " + this.age + " years");
    }

    // growOlder() method has been added
    public void growOlder() {
        this.age = this.age + 1;
    }
}
```

Just add a `public void` method to the class and use the `this` keyword to refer to the object (the instance).

![[Pasted image 20230517002648.png]]

**Return a value using a method**
___
Replace the `void` keyword in your method to a data type.

```java
public class Teacher {
    public int grade() {
        return 10;
    }
}
```

**The Complete Person Class**
___
```java
public class Person {
    private String name;
    private int age;

    public Person(String initialName) {
        this.age = 0;
        this.name = initialName;
    }

    public void printPerson() {
        System.out.println(this.name + ", age " + this.age + " years");
    }

    public void growOlder() {
        if (this.age < 30) {
            this.age = this.age + 1;
        }
    }
    // the added method
    public int returnAge() {
        return this.age;
    }
```

![[Pasted image 20230517004559.png]]

**Getters and Setters**
___
A method that returns the value of an instance variable is called a *getter*, and one that changes the value of an instance variable is called a setter.

**The `toString` method**
___
```java
public class Person {
    // ...

    public String toString() {
        return this.name + ", age " + this.age + " years";
    }
}
```

When you want to create a method that returns the instance variables of an instance, the convention is to call this method `toString`.

You can call the `toString` method of an instance variable simply by declaring the variable:

```java
System.out.println(antti);

// is extended by Java at run time to:

System.out.println(antti.toString());
```

**Calling instance methods within other instance methods**
___
```java
public String toString() {
    return this.name + ", age " + this.age + " years, my body mass index is " + bodyMassIndex();
}
```

You don't need to write `this.bodyMassIndex()` as Java knows this already.

**Calling instance variables within instance methods**
___
```java
public class PaymentCard {  
private double balance;  
public PaymentCard(double openingBalance) {  
// write code here  
this.balance = openingBalance;  
}  
  
public String toString() {  
// write code here  
return "The card has a balance of " + balance + " euros.";  
}  
}
```

You don't have to type `this.balance`, as Java will assume this already.



## Objects in a List
___
You can store objects of any one class in any one ArrayList.

For example you can store Persons:

```java
ArrayList<Person> persons = new ArrayList<>();
```



## Files and Reading Data
___
```java
// first
import java.util.Scanner;
import java.nio.file.Paths;

// in the program:

// we create a scanner for reading the file
try (Scanner scanner = new Scanner(Paths.get("file.txt"))) {

    // we read the file until all lines have been read
    while (scanner.hasNextLine()) {
        // we read one line
        String row = scanner.nextLine();
        // we print the line that we read
        System.out.println(row);
    }
} catch (Exception e) {
    System.out.println("Error: " + e.getMessage());
}
```

We use the Scanner class to read files.

This is the parameter given to create a new Scanner object:

```java
Paths.get("filename.extension")
```

What to do when there's an empty line in a file?
___

```java
// we create a scanner for reading the file
try (Scanner scanner = new Scanner(Paths.get("henkilot.csv"))) {

    // we read all the lines of the file
    while (scanner.hasNextLine()) {
        String line = scanner.nextLine();

        // if the line is blank we do nothing
        if (line.isEmpty()) {
            continue;
        }

        // do something with the data

    }
} catch (Exception e) {
    System.out.println("Error: " + e.getMessage());
}
```

`continue` skips the current iteration of the loop

`line.isEmpty()` is the conventional way to write `line.equals("")` 

