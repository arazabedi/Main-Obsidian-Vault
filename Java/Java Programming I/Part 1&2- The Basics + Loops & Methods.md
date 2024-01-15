#Java

**Print**
___
```java
System.out.println()
```
`shortcut: sout`

**Read (i.e. gets.chomp)**
___
```java
// import (utility) class
import java.util.Scanner;
// create instance of utility class
Scanner scanner = new Scanner(System.in);
// asign return of nextLine() method to a new variable
String message = scanner.nextLine();
// print input
System.out.println(message)
```

**String to int (e.g. "4" to 4")**
___
`Integer.parseInt()` when you know the input string represents an integer and you want to convert it to an `int` primitive type

**String to specific primitive (e.g. "4.2874" to 4.2874)** 
___
```java
Integer.valueOf()
Double.valueOf()
```

When you want to convert a string representation of a numeric value to its corresponding wrapper class object. You can parse an int as a double using this method.

**Wrapper Class Object**
___
In Java, a wrapper class is a class that encapsulates a primitive data type and provides additional functionality, such as converting the primitive data type to and from a string representation, or providing utility methods for working with the data type.

Each primitive data type has a corresponding wrapper class:

-   `boolean`: `Boolean`
-   `byte`: `Byte`
-   `short`: `Short`
-   `int`: `Integer`
-   `long`: `Long`
-   `float`: `Float`
-   `double`: `Double`
-   `char`: `Character`

A wrapper clas object allows you to use primitive data types as objects, rather than using them only for storing simple values like numbers or booleans.

**String to Boolean**
___
```java
Boolean.valueOf()d
```

**Division**
___
```java
int / int == int
```

```java
int / double == double
double / int == double
```

```java
(double) int / int == double
 int / (double) int == double
```
*See type-casting operations*

Division by 0 causes an error,  so use an if statement or try/catch to handle the exception.

Integer division truncates (rounds down). 

To round up a double, use:
```java
Math.round()
```

You can multiply by a double (1.0) in order to divide two integers as floats:

```java
double num = 1.0 * dividend / divisor;
```

What does the program below print?

```java
int dividend = 3;
int divisor = 2;

double result = dividend / divisor * 1.0;
System.out.println(result);
```

*The dividend / divisor part happens before the division, so the answer is 1. The float answer would be 1.5, but integer division is truncated down.*

**Conditional 'if' Statements**
___
```java
if (true) {
    System.out.println("This code is unavoidable!");
} else if (true) {
	System.out.println("This code is avoidable!");
} else (true) {
	System.out.println("This code is void!");
}
```
*Parentheses around the condition are necessary*

**String Comparison**
___
To check whether a string is the same as another, we do the following:

```java
randomString.equals("a string")
```

It will return true if the strings match.

**Java's Version of Until-Loop uses a 'true' While-Loop**
___
```java
int number = 1;

while (true) {
    System.out.println(number);
    if (number >= 5) {
        break;
    }

    number = number + 1;
}

System.out.println("Ready!");
```

*`true` is always true, so the while loop always runs until it hits the break command*

**Variables inside loops are private**
___
If you wish to use a variable after a loop, it needs to be introduced before the loop.

**For-loops**
___
Here is a while-loop, and then its for-loop equivalent:

```java
int i = 0;
while (i < 10) {
    System.out.println(i);
    i++;
}
```

```java
for (int i = 0; i < 10; i++) {
    System.out.println(i);
}
```

A `for` loop contains four parts: 
1.  A count variable (`i`)
2.  The condition of the loop (`i<10`)
3. The change in the counter on each loop (`i++`)
4. The functionality to be executed (`System.out.println(i)`)

```java
for (*introducing a variable*; *condition*; *increasing the counter*) {
    // Functionality to be executed
}
```

Loop conditions are only evaluated at the start of a loop
___
```java
int number = 1;

while (number != 2) {
    System.out.println(number);
    number = 2;
    System.out.println(number);
    number = 1;
}
```

The above `number` variable is at one point in each cycle meeting the condition for the end of the loop (`number == 1`), but it is returned to its other value of `1` before the loop is completed, so the condition is never evaluated by the loop and so it continues forever.

When to use a for, and when to use a while?
___
For-loops are good for predetermined numbers of loops.
While is better for undetermined number of loops.

While loop structure:
___
```java
Scanner reader = new Scanner(System.in);

// Create variables needed for the loop

while (true) {
    // read input

    // end the loop -- break

    // check for invalid input -- continue

    // handle valid input
}

// functionality to execute after the loop ends
```

Keep anything to be done after the loop has ended outside of your loop itself (including the variables to be printed or otherwise used elsewhere).

Programming in pieces
___
Always try to break down each part of a program into the smallest possible section. 

Complete each separate part, then move on to the next.

This avoids getting bogged down in understanding the full picture.


>[!tip]
>When you are writing a program, whether it's an exercise or a personal project, figure out the types of parts the program needs to function and proceed by implementing them one part at a time. Make sure to test the program right after implementing each part.
>
>Never try solving the whole problem at once, because that makes running and testing the program in the middle of the problem-solving process difficult. Start with something easy that you know you can do. When one part works, you can move on to the next.


Method Placement
___
In the code boilerplate, methods are written outside of the curly braces of the `main`, yet inside out the "outermost" curly braces. They can be located above or below the main.

```java
import java.util.Scanner;

public class Example {
    public static void main(String[] args) {
        Scanner scanned = new Scanner(System.in);
        // program code
    }

    // your own methods here
}
```

Methods are named statements to be used in your code.

Method Creation
___
```java
public static void greet() {
    System.out.println("Greetings from the method world!");
}
```

Chuck this into the above boilerplate:

```java
import java.util.Scanner;

public class Example {
    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);
        // program code
    }

    // your own methods here
    public static void greet() {
        System.out.println("Greetings from the method world!");
    }
}
```

>[!warning]
>Methods cannot be defined inside methods.

Public Static Void
___
"`public static void`" is a Java method signature, which specifies the accessibility, modifiers, and return type of a method.

-   "public" is an access modifier that allows the method to be accessed from any class in the same or other package.
-   "static" is a keyword that indicates that the method belongs to the class and not to any particular instance of the class. This means that the method can be called without creating an instance of the class.
-   "void" is the return type of the method, which means that the method does not return any value.

Therefore, "public static void" is commonly used to define a static method that can be called from any other class in the same or other package without creating an instance of the class, and that does not return any value.

**Method Parameters**
___
```java
public static void greet(int numOfTimes) {
    int i = 0;
    while (i < numOfTimes) {
        System.out.println("Greetings!");
        i++;
    }
}
```

**Arguments passed to methods are copies - not the original variable**
___
Certainly! In Java, when you pass an argument to a method, a copy of the argument's value is created and passed to the method. This means that any changes made to the parameter inside the method will only affect the copy of the value, not the original variable in the calling method.

**Methods Can Return Values**
___
Just change the `void` keyword. If you want to return an int:

```java
public static int
```

`void` means return nothing.

Use the `return` keyword:

```java
public static int alwaysReturnsTen() {
    return 10;
}
```

You must assign a return to a variable in order to use it:

```java
public static void main(String[] args) {
    int number = alwaysReturnsTen();

    System.out.println("the method returned the number " + number);
}
```

**`return` ends the method**
___
When a method execution reaches `return`, the method stops execution. 

You can use this to stop a void method without returning anything.

**Method variables are private**
___
You cannot use variables defined in a particular method elsewhere.