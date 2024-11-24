#### Main Method

Double check the final one.

```java
public static void // okay
static public void // okay
static public final void // okay
public static int // not okay
public void static // not okay
public static void Main // not okay - all lowercase
```

#### This

You CANNOT assign a value to `this`.

#### ArrayLists

ints are not implicitly converted to Double if an ArrayList expects a Double - when adding to an ArrayList/List you have to give the correct type.
#### Constructors
A constructor cannot be abstract, static, final, native, or synchronised.
A constructor cannot have a return type.

Call a same class constructor with this().
Call a parent constructor with super().

You can only call either this or super!!! Not both! Must be the first line of the constructor!!!!

#### Statics
Careful not to refer to instance variable from static context's.

#### Assigning statics to instances...

You can assign a static variable to an instance variable.
You can assign a instance variable to a static variable.
You can assign a local variable to an instance/static variable

#### Iterator
You cannot change an object which is being iterated on - (ConcurrentModificationException)

#### TryCatchFinally
If you have a return in try, or catch, but there is also a return in finally - FINALLY ALWAYS WINS.

Finally is always last. Catch after finally will result in a compilation error.

#### SOUT
Careful that you are not printing the reference - objects print a reference unless their toString() method is explicitly overwritten. 
(String class has an explicit toString() method so you get the value rather than the reference).

#### Initialisation
You need to initialise a local variable in order for it to be used.

BUT you don't need to initialise an instance or static varibale.

YOU MUST INITIALISE LOCAL VARIABLES.

#### Arrays
Arrays are MUTABLE - you can change the contents but not the size.

Arrays.sort() sorts alphabetically or numerically.

# READ CAREFULLY DON'T RUSH!!

#### Widening Casting

AUTOMATIC
- Only works for primitives
- `byte` -> `short` -> `char` -> `int` -> `long` -> `float` -> `double`

#### Narrowing Casting
MANUAL

**Data Types:**

- `short`: A 16-bit signed integer (-32,768 to 32,767).
- `char`: A 16-bit unsigned integer (0 to 65,535), used to represent Unicode characters.

When you convert a `short` to a `char`, you are moving from a signed integer to an unsigned integer. This conversion involves type casting, and there can be data loss if the `short` value is negative.

NARROWING = LOSING DATA

#### Lists

Array to List

```java
String[] games = {"Cricket", "Football"};
Arrays.asList(games)

List<String> list = new ArrayList<String>();  
Collections.addAll(list, games);
```
#### ArrayLists

ArrayLists can ONLY store OBJECTS. NOT PRIMITIVES.

Because ArrayLists use Generics.
#### String

Substring method takes the substring from index given as first argument (inclusive of that character) to the index given as second argument (if given - not required) exclusive of the character at the final index.

For example substring(6,12) would take the characters at index 6 to index 11.

If substring first argument index goes over the elements in the string, it returns an empty string.

```java
String s = "Hello World"
system.out.println(s.substring(6)) 
// output: World
```

String static methods: 
```java
valueOf() // calls toString()
format(), 
join(),
```

Format

```java
String formattedString = String.format("Name: %s, Age: %d", "Araz", 25); System.out.println(formattedString); // Output: "Name: Araz, Age: 25"
```

String formatted elements need to be in order & its a runtime error not compilation.

Join

```java
String joinedString = String.join(", ", "one", "two", "three");
System.out.println(joinedString);  // Output: "one, two, three"
```


```java
a = new String("Good Morning!");

// This creates 2 objects!
// new creates one in the heap
// The string literal creates another in the string pool
```
#### Memory - HEAP/STACK

HEAP - for primitives when class/instance/static variables (includes string pool)
STACK - for objects (primitives stored here when they are a local variable)

???? This not sure (memory part might be wrong).
### Math

- **Math.pow(double a, double b)**
    
    - **Parameters**: `double a`, `double b`
    - **Returns**: `double`
- **Math.sqrt(double a)**
    
    - **Parameters**: `double a`
    - **Returns**: `double`
- **Math.round(double a)**
    
    - **Parameters**: `double a`
    - **Returns**: `long`
    
    **Math.round(float a)**
    
    - **Parameters**: `float a`
    - **Returns**: `int`

Remember that numbers can be widened (e.g. give an int to pow it gets automatically caster to double).

MATH CLASS USES `double`

- **Math.floor(double a)**
    
    - **Parameters**: `double a`
    - **Returns**: `double`
- **Math.ceil(double a)**
    
    - **Parameters**: `double a`
    - **Returns**: `double`


Float implicitly casted to double.

#### Unreachable Code

This would cause a compilation error.

#### Binary Operator/Addition

```
a + b implicity coverts to int
```

#### Random

```
nextInt() // okay
nextDouble() // okay

next() // NOT OKAY COMPLETE BS
```

#### Variable Names

Variable names cannot start with a number!

#### Switch Statements

You need to have a break statement in each case otherwise the rest of the caseS will also run.

```java
switch(expression) {
  case x:
    // code block
    break;
  case y:
    // code block
    break;
  default:
    // code block
}
```

#### Initialisation

You have to initialise instance variables in the constructor unless already initialised.

#### Integer Division

```java
System.out.println(5/4); // (5/4) performs integer division because both 5 and 4 are integers, resulting in the value 1.
```

#### Binary Promotion

Why types will be promoted to int before performing mathematical operations?

byte, char and short

#### Floating Point Numbers

Numbers with decimals are automatically considered double unlesss specified using `1.0f`  or casting `(float)`.

#### ++ and --

In Java, the expression `System.out.println((a + b)-- * c);` contains a syntax error because you cannot apply the post-decrement operator `--` directly to the result of an arithmetic expression.

```java
int a = 5, b = 2, c = 30;  
System.out.println((a+b)-- * c);
// Compiler error.
```

#### Array

Size of the array is specified on the right not left.

#### Random

Math.random() (a function) is not Java.Util.Random (a class)

#### Final variable initialisation

Final instance variables MUST be initialised.

Static final must be initialised in declaration or in static block, not after declaration.


```java
public class MyClass {
    private static final int MY_STATIC_FINAL_INT = 42;
    private static final String MY_STATIC_FINAL_STRING = "Hello";

    public static void main(String[] args) {
        System.out.println(MyClass.MY_STATIC_FINAL_INT);    // Outputs 42
        System.out.println(MyClass.MY_STATIC_FINAL_STRING); // Outputs "Hello"
    }
}
```

```java
public class MyClass {
    private static final int MY_STATIC_FINAL_INT;
    private static final String MY_STATIC_FINAL_STRING;

    static {
        MY_STATIC_FINAL_INT = 42;
        MY_STATIC_FINAL_STRING = "Hello";
    }

    public static void main(String[] args) {
        System.out.println(MyClass.MY_STATIC_FINAL_INT);    // Outputs 42
        System.out.println(MyClass.MY_STATIC_FINAL_STRING); // Outputs "Hello"
    }
}
```

#### Unreachable Code

You CANNOT have unreachable code in a while loop.

You can in an if statement (exception to the rule).

A for loop can be tricked into allowing unreachable code by using an unfulfilled condition.

#### StringBuffer Class

The StringBuffer class is a mutable String class.

You cannot compare a String and a StringBuffer using `.equals()` as they are not convertible.

#### TRYCATCH

A try/catch block needs a try, and then a catch or a finally (you can forgoe one or the other).

You can nest try/catch

#### Truthiness

There is no truthiness/falsiness in Java.

An if statement requires a boolean. If you use an expression it must evaluate specifically to a boolean. Positive numbers for example do not automatically get treated as true - the code won't compile.

#### Public Class

There can only be one public class in a Java source code file!

#### Integer.valueOf() vs Integer.parseInt()

Integer.valueOf(): returns Integer (wrapper class)
Integer.parseInt(): returns primitive int

#### Interface Variables
Are implicitly `public static final`

#### Multiple variables in a for loop

CORRECT:
```java
for (int i = 0, k = 0 ; i<=10 ; i++) {
...
}
```

NOT: 

```java
for (int i = 0, int k = 0 ; i<=10 ; i++) {
...
}
```

(declare type once, use comma to add more variables).

#### Exceptions in Catch Blocks

An exception thrown in a catch block will not be caught by other catch blocks. 

Catch only catches try.

#### Switch

You cannot use non-final variables as `case` labels in a Java `switch` statement. The `case` labels must be constant expressions, which means they must be known at compile-time and cannot change. This is why you can only use literals, `final` variables, or enum constants as `case` labels.

Only integral types are allowed in `switch` statements. These include `int`, `byte`, `short`, `char`, and their corresponding `Integer`, `Byte`, `Short`, and `Character` wrapper classes, as well as `String` and `enum` types.

#### Char and Short

You need to explicitly cast short into int and int into short because you might lose data (signed/unsigned) so the compiler doesn't automatically do it itself.

#### Dangling Else

If there are no curly braces in an if statement, the else clause is associated with the closest if (unless that if already has an else clause).

(Usually inner if gets the if).

#### Implicit Narrowing


