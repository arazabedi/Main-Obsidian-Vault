## Objects on a List and a List as Part of an Object
___
You can create a class of objects that contain a list, as below:

```java
// imports

public class Playlist {
    private ArrayList<String> songs;

    public Playlist() {
        this.songs = new ArrayList<>();
    }

    public void addSong(String song) {
        this.songs.add(song);
    }

    public void removeSong(String song) {
        this.songs.remove(song);
    }

    public void printSongs() {
        for (String song: this.songs) {
            System.out.println(song);
        }
    }
}
```

```java
Playlist list = new Playlist();
list.addSong("Sorateiden kuningas");
list.addSong("Teuvo, maanteiden kuningas");
list.printSongs();

//Sorateiden kuningas 
//Teuvo, maanteiden kuningas
```

**Stacks**
___
A stack is a data structure similar to an array, except that you always add or take (push or pop) to or from the top.

**Objects in an Instance Variable List**
___
You can put any type of object in a list that is an instance variable.

This includes self-made classes.

**The Join Method**
___
`String.join(separator, Array/ArrayList)`

This will take your array, and print out each element of the array with the separator in between each element.

The Clear Method
___
To delete all list elements, call `clear` on the list.

**Retrieving a Specific Object from a List**
___
1. Check to see if the list contains anything. Return `null` if not
2. Create a variable and put the first element in it.
3. Go through every element and if it's greater than the existing held object, replace it.
4. Return the variable

```java
public Person getTallest() {
    // return a null reference if there's no one on the ride
    if (this.riding.isEmpty()) {
        return null;
    }

    // create an object reference for the object to be returned
    // its first value is the first object on the list
    Person returnObject = this.riding.get(0);

    // go through the list
    for (Person prs: this.riding) {
        // compare each object on the list
        // to the returnObject -- we compare heights
        // since we're searching for the tallest,

        if (returnObject.getHeight() < prs.getHeight()) {
            // if we find a taller person in the comparison,
            // we assign it as the value of the returnObject
            returnObject = prs;
        }
    }

    // finally, the object reference describing the
    // return object is returned
    return returnObject;
}
```

## Separating the User Interface from Program Logic
___
Try to encasulte the 'dirty details' of a concept inside an object class. For example, if you need a list instance variable in a class, but you also need to have some specific logic for that list, make a separate class for that list.

The example given by MOOC has to do with deparating a `WordSet`from a `UserInterface` class.

Similarly to using components in React, try to separate out concerns so that your code is more readable.

The key takeaway is that if we want to change a particular class (just like a React component), it won't affect other classes it isn't a parent of. Therefore we can tinker with things without breaking the whole programme.

The `UserInterface` class uses `WordSet` through its public interfaces - it methods.

```java
import java.util.ArrayList;

public class WordSet {
    private ArrayList<String> words

    public WordSet() {
        this.words = new ArrayList<>();
    }

    public void add(String word) {
        this.words.add(word);
    }

    public boolean contains(String word) {
        return this.words.contains(word);
    }
```

```java
public class UserInterface {
    private WordSet wordSet;
    private Scanner scanner;

    public userInterface(WordSet wordSet, Scanner scanner) {
        this.wordSet = wordSet;
        this.scanner = scanner;
    }

    public void start() {

        while (true) {
            System.out.print("Enter a word: ");
            String word = scanner.nextLine();

            if (this.wordSet.contains(word)) {
                break;
            }

            this.wordSet.add(word);
        }

        System.out.println("You gave the same word twice!");
    }
}
```

```java
public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    WordSet set = new WordSet();

    UserInterface userInterface = new UserInterface(set, scanner);
    userInterface.start();
}
```

**Implementing new functionality**
___
If you wanted to add new functionality to the back-end (the class that deals with the set of words), you can add a method to the `WordSet` class, and simply invoke it in the front-end (the `UserInterface` class).

![[Pasted image 20230603131519.png]]

**From one entity to many parts**
___
You could put all your code for a programme into `main`.

This would be hard to maintain.

Instead, identify several discrete areas of responsibility within the programme. For example, if you are making a programme that take user input grades, then output a star rating based on those grades, you could separate out the conversion process and keeping track of the grades into a different class. You could also separate out the user interface.


## Introduction to Testing
___
**Stack Trace**
___
When an error occurs in a programme, it typically prints a stack trace - a list of method calls that resulted in the error.

```java
Exception in thread "main" ... at Program.main(Program.java:15)
```

The above tells us that the error occured at line 15 of Program.java.

![[Pasted image 20230603232000.png]]

**Passing Test Input to Scanner**
___
```java
String input = "one\n" + "two\n"  +
                "three\n" + "four\n" +
                "five\n" + "one\n"  +
                "six\n";

Scanner reader = new Scanner(input);
```

Use the Scanner class's constructor to directly input to the scanner without actually doing so in the console.

Use the "\n" to simulate a new line for the `scanner.nextLine()` method.

**Unit Testing**
___
Small parts of a programme are tested in isolation.

These parts can be classes, for example.

JUnit is the most common ready-made testing library.

We have this class:

```java
public class Calculator {

    private int value;

    public Calculator() {
        this.value = 0;
    }

    public void add(int number) {
        this.value = this.value + number;
    }

    public void subtract(int number) {
        this.value = this.value + number;
    }

    public int getValue() {
        return this.value;
    }
}
```

The subtract method is incorrect.

To test, create a test class:

```java
public class CalculatorTest {

}
```

The naming is important.

Next, let's test that the calculator has an initial value of 0:

```java
import static org.junit.Assert.assertEquals;
import org.junit.Test;

public class CalculatorTest {

    @Test
    public void calculatorInitialValueZero() {
        Calculator calculator = new Calculator();
        assertEquals(0, calculator.getValue());
    }
}
```

The `assertEquals()` method returns true if the variable returned by the second parameter is equal to the first parameter.

Remember to import the Test framework, as well as the specific test method you need (as static).

Each test has an `@Test` annotation which tells the test framework that this is an executable test method.

Run the tests by right-clicking and pressing Test.

This will print output specific to each test class.

```java
Testsuite: CalculatorTest Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.054 sec

test-report: test: BUILD SUCCESSFUL (total time: 0 seconds)
```

Add more functionality to the test class:

```java
import static org.junit.Assert.assertEquals;
import org.junit.Test;

public class CalculatorTest {

    @Test
    public void calculatorInitialValueZero() {
        Calculator calculator = new Calculator();
        assertEquals(0, calculator.getValue());
    }

    @Test
    public void valueFiveWhenFiveAdded() {
        Calculator calculator = new Calculator();
        calculator.add(5);
        assertEquals(5, calculator.getValue());
    }

    @Test
    public void valueMinusTwoWhenTwoSubstracted() {
        Calculator calculator = new Calculator();
        calculator.subtract(2);
        assertEquals(-2, calculator.getValue());
    }
}
```

This will output: 

```java
Sample output

Testsuite: CalculatorTest Tests run: 3, Failures: 1, Errors: 0, Skipped: 0, Time elapsed: 0.059 sec

Testcase: valueMinusTwoWhenTwoSubstracted(CalculatorTest): FAILED expected:<-2> but was:<2> junit.framework.AssertionFailedError: expected:<-2> but was:<2> at CalculatorTest.valueMinusTwoWhenTwoSubstracted(CalculatorTest.java:25)

Test CalculatorTest FAILED test-report: test: BUILD SUCCESSFUL (total time: 0 seconds)
```

Three tests were executed. One failed. The line of the failed test is 25. The expected was -2 but it got 2.

Fix the issue and you'll get:

```java
Testsuite: CalculatorTest
Tests run: 3, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.056 sec

test-report:
test:
BUILD SUCCESSFUL (total time: 0 seconds)
```

>[!warning]
>Writing tests for code written in `main` is very difficult. Make sure to separate concepts out using classes.

**Test-Driven Development**
___
Within TDD, software is developed in small chunks.

The first thing a programmer does is write an automatically-executable test which tests a single piece of the programme.

The test will of course fail because no programme code has yet been written.

Once the test has been written, the functionality required to pass it is added to the programme and the tests are run again.

Once the tests are passed, a new test is added, and so on.