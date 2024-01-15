#Java
## Programming Paradigms
___
The most common programming paradigms today are object-oriented programming, procedural programming, and functional programming.

Most programming languages currrently in use support multiple programming paradigms.

**Object-Oriented Programming**
___
Information is represented as classes that describe the concepts of the problem domain and the logic of the application.

Classes define the methods that determine how information is handled.

During program execution, objects are instantiated from classes that contain runtime information and that also have an effect on programme execution.

Programme execution typically proceed through a series of method calls related to the objects.

The programme is built from small, clear, and cooperative entities.

OOP first appeared in Simula 67 and Smalltalk. It became ubiquitous after the introduction of C++ and later Java.

A major benefit of OOP is how problem-domain concepts are modeled through classes and objects, which makes programmes easier to understand.

Additionally, this structuring facilitates the construction and maintenance of programmes.

Howeverm OOP is not suited to all problems.

Scientific computing and statistics applications typically make use of languages such as R and Python.

**Procedural Programming**
___
In OOP, the structure of a programme is formed by the data it processes. 

In procedural programming, the structure of the programme is formed by functionality desired for the programme.

It acts as a step-by-step guide for the functionality to be performed.

The programme is executed one step at a time, and subroutines (methods) are called whenever necessary.

In procedural programming, the *state* of the programme is maintained in variables and tables. Any methods handle only the values provided to them as parameters.

```java
int a = 10;
int b = 15;

// let's swap the values of variables a and b
int c = b;
b = a;
a = c;
```

In OOP, state can change with any object method, and that change of state can affect the working of methods of other objects.

Other aspects of a programmes execution may also be affected since objects can be used in multiple places within the programme.

Procedural Clock:
```java
int hours = 0;
int minutes = 0;
int seconds = 0;

while (true) {
    // 1. printing the time
    print(hours, minutes, seconds);
    System.out.println();

    // 2. advancing the second hand
    seconds = seconds + 1;

    // 3. advancing the other hands when necessary
    if (seconds > 59) {
        minutes = minutes + 1;
        seconds = 0;

        if (minutes > 59) {
            hours = hours + 1;
            minutes = 0;

            if (hours > 23) {
                hours = 0;
            }
        }
    }
}
```

```java
public static void print(int hours, int minutes, int seconds) {
    print(hours);
    print(minutes);
    print(seconds);
}

public static void print(int value) {
    if (value < 10) {
        System.out.print("0");

    System.out.print(value);
}
```

OOP Clock:

```java
public class Hand {
    private int value;
    private int upperBound;

    public Hand(int upperBound) {
        this.upperBound = upperBound;
        this.value = 0;
    }

    public void advance() {
        this.value = this.value + 1;

        if (this.value >= this.upperBound) {
            this.value = 0;
        }
    }

    public int value() {
        return this.value;
    }

    public String toString() {
        if (this.value < 10) {
            return "0" + this.value;
        }

        return "" + this.value;
    }
}
```

```java
public class Clock() {
    private Hand hours;
    private Hand minutes;
    private Hand seconds;

    public Clock() {
        this.hours = new Hand(24);
        this.minutes = new Hand(60);
        this.seconds = new Hand(60);
    }

    public void advance() {
        this.seconds.advance();

        if (this.seconds.value() == 0) {
            this.minutes.advance();

            if (this.minutes.value() == 0) {
                this.hours.advance();
            }
        }
    }

    public String toString() {
        return hours + ":" + minutes + ":" + seconds;
    }
}
```

```java
Clock clock = new Clock();

while (true) {
    System.out.println(clock);
    clock.advance();
}
```

Programming functionality procedurally is an actual nightmare. Just do it OOP.

## Algorithms
___
Algorithms are precise instructions on how to accomplish a specific task.

In programming, they are defined using code.

The concept of efficiency is often associated with algorithms, as this is an integral part of a programme's usability.

Here we will explore algorithms associated with retrieving and sorting information.

##### Sorting Information
___
If information given to a computer is unordered and rule-free, it is very taxing for the computer to retrieve that information. We need order!

Sorting algorithms are ways to sort an array.

The `Arrays` class has a `sort` method, as does the  `Collections` class (the latter works for `ArrayList`).

```java
Arrays.sort(arr)
Collections.sort(arr)
```

**Selection Sort**
___
For each index of the array, compare it to each other element of the array and swap them if the compared element is smaller (this would allow for multiple swaps for each index).


##### Information Retrieval
___
There are numerous ways to search for a particular piece of information. Here we will mention two of them.

**Linear Search**
___
Go through every single value in an array one by one then return the index immediately once the correct value is reached.

If nothing is found, return -1.

```java
public class Algorithms {

    public static int linearSearch(int[] array, int searched) {
        for (int i = 0; i < array.length; i++) {
            if (array[i] == searched) {
                return i;
            }
        }

        return -1;
    }
}
```

**Binary Search**
___
Also known as 'half-interval search or logarithmic search'. 

This requires the data to be ordered.

It is much more efficient than linear search.

You start looking for the searched value in the middle index of the array. Compare the values, then eliminate half the values. Rinse and repeat.



