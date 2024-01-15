#Java
## ArrayList
___
A Java ArrayList is a resizable array data structure. You can import using the following:

```java
import java.util.ArrayList;
```

ArrayList is faster for adding and removing elements, but slower than Array for accessing elements by index. This is because Array has a fixed size. Memory usage is higher for ArrayList.

Use ArrayList when lots of data needs to be stored, added, and deleted. Use Array when a known quantity of elements need to be accessed by index quickly.

**ArrayList is a Class**
___
It is a class that implements the List interface. The List interface defines a set of methods for working with lists of objects.

Interfaces and Interface Implementation
___
An interface in OOP languages is a contract that specifies the methods that a class must have, without specifying how those methods should be implemented.

```java
interface Animal { 
	void makeSound(); 
	void eat(); 
}
```

Any class that implements the animal interface must have these two methods.

Use the `implements` keyword to do this:

```java
class Dog implements Animal {
  @Override
  public void makeSound() {
    System.out.println("Woof!");
  }

  @Override
  public void eat(String food) {
    System.out.println("I'm eating " + food);
  }
}
```

**Create a List**
___
You must specify the type of stored elements within the angle brackets after the ArrayList:

```java
ArrayList<String> list = new ArrayList<>();
```

Whatever you put in the angle brackets must be a class (capitalised). You cannot use `int` or `double`, you must use `Integer` or `Double`. These are utility classes that extend the basic data types.

You can add `int` and `double` variables to an `Integer` or `Double` list as Java automatically converts these.

**Add to and retrieve from a list**
___
```java
import java.util.ArrayList;

public class WordListExample {

    public static void main(String[] args) {
        // create the word list for storing strings
        ArrayList<String> wordList = new ArrayList<>();

        // add two values to the word list
        wordList.add("First");
        wordList.add("Second");

        // retrieve the value from position 0 of the word list, and print it
        System.out.println(wordList.get(0));
    }
}
```

Non-existent index error
___
```java
IndexOutOfBoundsException
```

If your `.get` parameter is a number which does not match an index in your list, you will get this error.

**List Size**
___
```java
list.size()
```

**Use List Size to Get the Last Index**
___
There isn't a `list[-1]` feature so you have to use:

```java
list.get(list.size()-1)
```

**Use a For-Loop to Iterate Through a List**
___
There is a `forEach()` method introduced in Java 8, but a for loop allows you to access the index of the current element, allowing for more complex iteration logic. 

**Iterating Over a List with a For-Each Loop**
___
If you don't need access to the index, use a for-each loop:

```java
ArrayList<String> teachers = new ArrayList<>();


teachers.add("Simon");
teachers.add("Samuel");
teachers.add("Ann");
teachers.add("Anna");

for (String teacher: teachers) {
    System.out.println(teacher);
}
```

**Remove from a List**
___
```java
list.remove(0)
```

You can use the index to remove an element, or also the element itself:

```java
ArrayList<String> list = new ArrayList<>();

list.add("First");
list.add("Second");
list.add("Third");

list.remove("First");

System.out.println("Index 0 so the first value: " + list.get(0));

// prints: Index 0 so the first value: Second
```

You cannot do this if the elements themselves are integers, as this contradicts the use of indexes (which are also integers).

Essentially index has priority.

To circumvent this issue, use `Integer.valueOf()`:

```java
ArrayList<Integer> list = new ArrayList<>();

list.add(15);
list.add(18);
list.add(21);
list.add(24);

list.remove(2);
list.remove(Integer.valueOf(15));

System.out.println("Index 0 so the first value: " + list.get(0));
System.out.println("Index 1 so the second value: " + list.get(1));

// prints:
// Index 0 so the first value: 18 
// Index 1 so the second value: 24
```

**Check the Existence of a Value in a List**
___
The `.contains` method returns a boolean.

```java
list.contains()
```

```java
ArrayList<Integer> list = new ArrayList<>();

list.add("First")

System.out.println(list.contains("First"))

// prints:
// true
```

**Methods Can Take a List as a Parameter**
___
```java
public static void printSmallerThan(ArrayList<Integer> numbers, int threshold) {
    for (int number: numbers) {
        if (number < threshold) {
            System.out.println(number);
        }
    }
}
```

Modifying a List in a Method Affects the Original List
___
When you pass a list or any other reference-type variable to a method in Java, any changes made to that variable inside the method will also affect the original variable outside the method.

Why?

The reason for this behavior is that in Java, reference-type variables (such as lists) are actually references to objects in memory, rather than values. When you pass a reference-type variable to a method, the method receives a copy of the reference, but both the original variable and the copy still refer to the same object in memory.

Therefore, any changes made to the object through the reference in the method will also be reflected in the original variable outside the method, since they both refer to the same object in memory.

On the other hand, when you pass a value-type variable (such as an int) to a method, the method receives a copy of the value itself, rather than a reference to the variable. Therefore, any changes made to the value inside the method do not affect the original variable outside the method.

Basically - reference variables can be changed in a method, but value variables cannot.

**End Java Method Execution**
___
```java
    public static void removeLast(ArrayList<String> strings) {
        if (strings.size() == 0) {
            return;
        } else {
            strings.remove(strings.size() - 1);
        }
    }
```

Use the `return` keyword.


## Array
___
Unlike an ArrayList, an Array has a limited number of spots.

All these spots must be filled.

For instances where you don't yet want to fll those spaces you can use the integer 0, for example.

**Creating an Array**
___

```java
int[] numbers = new int[3];
String[] strings = new String[5];
typeDeclaration nameOfVariable = new Type[sizeOfArray]
```

Accessing and Assigning Array Elements
___
It's the same as any other language:

```java
int[] numbers = new int[3];
numbers[0] = 2;
numbers[2] = 5;

System.out.println(numbers[0]);
System.out.println(numbers[2]);
```

Array Length
___
Arrays do not have a `.size()` method. Instead, use:

```java
array.length
```

*note the absense of brackets*

`.length` gives you access to the array's length property, rather than executing a method to calculate the length.

Iterating Through an Array
___
You can use a while loop to iterate through an array.

**Shortcut to Creating an Array**
___
```java
int[] numbers = {100, 1, 42};

String[] arrayOfStrings = {"Matti L.", "Matti P.", "Matti V."};

double[] arrayOfFloatingpoints = {1.20, 3.14, 100.0, 0.6666666667};
```

*The curly braces signify a block*


## Using Strings
___
We've already seen concatenation (`+`):

```java
String word = "Hello";
System.out.println(word + word + word);
// prints:
HelloHelloHello
```

We've also seen string comparison (`.equals`):

```java
String text = "course";
String anotherText = "horse";

if (text.equals(anotherText)) {
    System.out.println("The two texts are equal!");
} else {
    System.out.println("The two texts are not equal!");
}
```

*A string variable must have some value or else you'll get the `_NullPointerException_` error.*

**Splitting a String**
___
```java
String string = "Hello world";
String[] splitArray = string.split(" ");
System.out.println(splitArray[0])
System.out.println(splitArray[1])
//prints:
// Hello
// world
```

`.split` returns an array with the elements before and after the splitting parameter. In the above case, the string is split by a space, so the words remain as elements of the new array.

Checking to See if a String Contains Another String
___
```java
String string = "Hello"
System.out.println(string.contains("el"))
// prints
// true
```

Find the Character at a Particular Index
___
Use the `charAt()` method on a string:

```java
String text = "Hello world!";
char character = text.charAt(0);
System.out.println(character);
```

Find the Length of a String
___
```
string.length()
```

*Note that this is a method and thus requires parentheses*