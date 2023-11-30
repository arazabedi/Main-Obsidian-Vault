## Learning OOP
___
**Object Oriented Programming**:

**Object-oriented programming is primarily about isolating concepts into their own entities or, in other words, creating abstractions**.

You can abstract the operations of a method behind the method call. No one has to see the method code in order to call the method and make use of it.

A class may be used in a polymorphistic way - for example a class that can be used to implement a clock hand could also be used to implement any counter limited by a particular integer.

We can also use the inheritance principle of classes to use characteristics of a parent class within a child class. For example a Clock class inherits the `advance()` method from the ClockHand class.

All of the characteristics of an object, for example, a clock, are encapsulated by the class. The characteristics are the attributes and methods. Encapsulation helps in achieving data security and code maintainability.

In OOP, a program is built from small and distinct objects that work together.

**Objects**
___
An object contains instance variables and instance methods (data and behaviour respectively).

Some objects are used to describe a problem, while others coordinate the interaction of other objects.

Object interaction occurs through method calls.

An object hides its internal operations, and simply provides access to its functionality through clearly defined methods.

Objects are also independent of all other objects which it doesn't require to accomplish its task.

Object ***state*** consists of the values of its instance variables at any particular point in time.

**Class**
___
A class is a blueprint from which objects are created.

It contains the object's data (instance variables), a constructor used to create the object (or multiple constructors), and methods that define the object's behaviour. A class may also contain methods that are called on the class itself.

Use the `new` command to create a new object from a class.

##  Removing Repetitive Code (overloading methods and constructors)
___
**Constructor Overloading:**

Constructor overloading is when you have more than one constructor in a class.

```java
public Person(String name) {
        this.name = name;
        this.age = 0;
        this.weight = 0;
        this.height = 0;
    }

public Person(String name, int age) {
    this.name = name;
    this.age = age;
    this.weight = 0;
    this.height = 0;
}
```

The above two constructors differ in that the top one need only a name parameter, while the bottom also has an age parameter which allows you to set the age of the new object, which isn't possible with the top constructor.

>[!warning]
>A class can have multiple constructors that differ in the number and/or type of their parameters. It's not, however, possible to have two constructors with the exact same parameters.
>
>For example, you cannot add `public Person(String name, int weight)` to the above class, as it is indifferentiable to the other constructor with a String and an int parameter.
>

Calling a Constructor
___
```java
public Person(String name) {
    this(name, 0);
    //here the code of the second constructor is run, and the age is set to 0
}

public Person(String name, int age) {
    this.name = name;
    this.age = age;
    this.weight = 0;
    this.height = 0;
}
```

With the above method, you can call the second constructor within the first, but set the age to 0.

This is because the first constructor is a special subset of the first.

`this` in this context is used to invoke the second constructor, and does to not refer to any object instance.

**Method Overloading**
___
You can also overload methods in order to give defaults to paramaters when you call the method without giving an argument to the parameter:

```java
public void growOlder() {
    this.age = this.age + 1;
}

public void growOlder(int years) {
    this.age = this.age + years;
}
```

You can also do this using the `this` keyword:

```java
public void growOlder() {
    this.growOlder(1);
}

public void growOlder(int years) {
    this.age = this.age + years;
}
```

*Note that in this case, `this` does in fact refer to the object, upon which the `growOlder()` method is invoked. This is slightly different to the constructor overloading, which uses `this` to refer to the parent constructor.*

## Primitive and Reference Variables
___

Java variables are either ***primitive*** or ***reference***.

A primitive variable contains a value.

A reference variable contains a reference to some other store of a value (either an object or another data structure).

Reference variables point in the direction of the memory location where the actual value is stored.

Reference variables are used with non-primitive types such as classes, interfaces, arrays etc.

```java
int age = 25; // primitive variable
String name = "John"; // reference variable
```

Primitives
___
When you declare `int age` in Java, it instructs the language to reserve space for storing an `int` value.

When you assign the value 25 to the `age` variable, a specific amount of memory is allocated to store this integer value, and the value 25 is assigned to that allocated memory location.

Using `age` in Java code allows for direct access to the value held within the memory location represented by `age`.

References
___
When you declare `String name`  in Java, reference variable in Java, you are instructing Java to reserve space for storing a reference to an object or another data structure.

Memory is reserved for a reference variable named `name` but memory is not allocated at this point.

Once you assign the reference variable `name` a value of "John",  memory is allocated to the String object which is referenced by the reference variable `name`.

>[!warning]
>If a class has no `toString` method, then the default outputted string (when you `sout` the variable) will look something like:
>
>`Name@4aa298b7`
>
>This consists of the name of the class(`Name`), and identifier of the variable(`4aa298b7`).

**Primitive Variables**
___
Java has 8 different primitive variables.

- `boolean - true/false`
- `byte - -128 to 127`
- `char - a single character (16-bit)`
- `short -32768 to 32767 (16-bit)`
- `int -2^31 to 2^31-1 (32-bit)`
- `long -2^63 to 2^63-1 (64-bit) 
- `float 32-bit floating-point number`
- `double 64-bit floating-point number``

Declaring a primitive variable causes the computer to reserve some memory where the value assigned to the variable can be stored.

The size of the storage conainer reserved depends on the type of the primitive.

The name of the variable gives the memory location where its value is stored.

When you assign a value to a primitive variable, the variable on the right-hand side of the equals sign is copied to the memory location indicated by the name of the variable.

Whenever a variable is used in a method call, the value is copied.

This is why variables used in methods are private. A method variable has nothing to do with a variable with the same name that is outside that variable, even if the variable is assigned a value through a parameter given an identically named variable.

**Reference Variables**
___
All Java variables bar the primitives are of the reference type.

This includes all those given by the Java language itself, any libraries you may encounter, and any variables that you create yourself (that are not primitives).

Any object instanced from a class is a reference variable.

Take the following:

```java
Name leevi = new Name("Leevi");
```

We declare a type, in this case the calss `Name`:

```java
Name ...
```

We state the name of the reference variable, which can reference the value of the variable later on:

```java
Name leevi...
```

We assign the value "Leevi" to the reference variable.

```java
... new Name("Leevi");
```

The constructor call returns a value that is a reference to the newly-created object. The reference is a memory address like we've seen above.

That reference is assigned to the variable `leevi`.

The reference variable `leevi` can now be used to access and manipulate the properties and behaviour of the object created by this code:

```java
Name leevi = new Name("Leevi");
```

Primitives are immutable, but the state of reference variables can usually be mutated.

When you apply arithmetic operations to primitive variables, the original values of the variables are not changed. Rather, the new values are stored in the variables as needed.

The values of reference variables cannot be changed by arithmetic expressions. However the internal instance variable values of objects pointed to by reference variables can indeed by altered.

**Primitive and Reference Variable as Method Parameters**
___
When passing a primitive or reference as an argument to a method, the value of the argument is copied for the method to use.

>[!info]
>Remember that while primitive variables hold actual values, reference variables only hold a reference to an object or data type, and it is this reference that will be copied.


## Objects and References
___
If you create a new Person variable and assign it the Person variable `joan`, what happens?

```java
Person joan = new Person("Joan Ball");
System.out.println(joan);

Person ball = joan;
```

Since `joan` is a reference variable whose value is just a reference, the Person `ball` is now also a reference variable whose value is just a reference.

In fact, it is the same reference!

`joan` and `ball` both refer to the same object.

What if you re-assign a variable?

```java
Person joan = new Person("Joan Ball");
System.out.println(joan);

Person ball = joan;
ball.growOlder();
ball.growOlder();

System.out.println(joan);

joan = new Person("Joan B.");
System.out.println(joan);
```

```bash
Joan Ball, age 0 years 
Joan Ball, age 2 years 
Joan B., age 0 years
```

Since `joan` is assigned to the value of a new constructor call, a new object reference is held as a value within `joan`.

**`null` as the value of a reference variable**
___
Printing a null reference prints "null".

You cannot call a method on a null value.

`null` is a special value that represents the absence of an object reference. `null` is not an instance of a class and does not have any methods associated with it.

You will get a `NullPointerException` at runtime if you call a method on a null variable.

**Objects as method arguments**
___
When you pass an object as an argument to a method, the reference is copied, and so you can access the objects data and behaviour using the variable name.

**Objects as instance variables**
___
Instance variables can of course contain references to objects.

For example, a `birthday` variable could contain a `SimpleDate` object reference.

**Objects of the same type as method arguments**
___
Comparing two objects of the person class can be done by using a `getAge()` method and comparing the two ages.

A more 'object-oriented' approach is to include a comparison method for all objects of a class.

`olderThan()` would give you a boolean depending on the outcome of the comparison.

**Abstraction**
___
Each concept has its own clear responsibilities. 

For example, a comparison of age may be better placed within a date/time handling class, as that is where the concept of time is held.

**Encapsulation**
___
When object (instance) variables are `private`, they are encapsulated. 

Yet we can read their values through methods.

`private` variables can only be accessed from the object methods of their class.

**Checking object equality**
___
Two `int` variables, or any other two primitives of the same type, can be checked for equality using the equals (`=`) symbol.

This is not the case for reference variables, for which you need a method in order to invoke this functionality.

The String method `equals` is similar to the method `toString` in that it is available for use without needing to be defined in a class.

The default implementation of `equals` checks the equality of the references of reference variables.

It would only return false if the variables refer to the exact same object.

If you want to compare actual values (i.e. primitives) that are held within objects as the values of instance variables, then you have to explicitly code an equals method in the class.

```java
public boolean equals(Object compared) {
        // if the variables are located in the same position, they are equal
        if (this == compared) {
            return true;
        }

        // if the type of the compared object is not SimpleDate, the objects are not equal
        if (!(compared instanceof SimpleDate)) {
            return false;
        }

        // convert the Object type compared object
        // into a SimpleDate type object called comparedSimpleDate
        SimpleDate comparedSimpleDate = (SimpleDate) compared;

        // if the values of the object variables are the same, the objects are equal
        if (this.day == comparedSimpleDate.day &&
            this.month == comparedSimpleDate.month &&
            this.year == comparedSimpleDate.year) {
            return true;
        }

        // otherwise the objects are not equal
        return false;
    }
```

Essentially, an overriden `equals` method should include:

1. `true` if object is same as compared
2. `false` if compared is not of the same class
3. `true` if all the instance variables are the same

IntelliJ has an automatic `equals` feature which gives this code for the Person class:

```java
public boolean equals(Object o) {  
if (this == o) return true;  
if (o == null || getClass() != o.getClass()) return false;  
Person person = (Person) o;  
return height == person.height && weight == person.weight && Objects.equals(name, person.name) && Objects.equals(birthday, person.birthday);  
}
```

**The Object Class**
___
Every class we create and all ready-made Java classes inherit the class `Object`. An instance of any class can therefore be passed as a parameter to a method than receives an Object type variable.

The effect of the `Object` class is also apparent as methods such as `toString` and `equals` are usable for all classes, since they belong to originator Object class.

**Object quality and lists**
___
Scenario:
- A list contains a `Bird` object, through a variable named red, with the name "Red"
- The variable red is re-assigned a new `Bird` object with the name "Red"
- You check to see whether the list contains the variable red
- `birds.contains(red)` will be false, because the two objects are separate, even though they contain the same instance variable values.

If you want the equality for birds to come true when the name of the birds is the same, then you must code an `equals` method yourself.

```java
public boolean equals(Object compared) {
        // if the variables are located in the same position, they are equal
        if (this == compared) {
            return true;
        }

        // if the compared object is not of type Bird, the objects are not equal
        if (!(compared instanceof Bird)) {
            return false;
        }

        // convert the object to a Bird object
        Bird comparedBird = (Bird) compared;

        // if the values of the object variables are equal, the objects are, too
        return this.name.equals(comparedBird.name);

        /*
        // the comparison of names above is equal to
        // the following code

        if (this.name.equals(comparedBird.name)) {
            return true;
        }

        // otherwise the objects are not equal
        return false;
        */
    }
```

Now, the `contains` method of `ArrayList` will recognise the equality of two Bird objects with the same name, as the `contains` method of the `ArrayList` class uses the `equals` method of the Bird class itself.

>[!warning]
>It's quite easy to forget that you *MUST* compare Strings with `equals`. 

>[!warning]
> You can't use `break` inside an `if` statement

**Returning objects from methods**
___
Just remember that new objects with the same instance variables are indeed separate objects with different references.

The `final` keyword
___
```java
public class Money {

    private final int euros;
    private final int cents;

    public Money(int euros, int cents) {
        this.euros = euros;
        this.cents = cents;
    }

    public int euros() {
        return euros;
    }

    public int cents() {
        return cents;
    }

    public String toString() {
        String zero = "";
        if (cents <= 10) {
            zero = "0";
        }

        return euros + "." + zero + cents + "e";
    }
}
```

Using the word `final` in an instance variable means that once it has been set by the constructor, it cannot be changed.