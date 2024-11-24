#### Week 1
##### Day 1
Academy ends Friday 5th July

##### Day 2
Software Development Life Cycle
Domain Modelling
Creating user stories, domain models and Kanban boards
Jonathan Perez will be dealer for poker
Jordan Harwood-Griffith likes Tame Impala and Bleach/One Piece but defo plays too many video games

>[!Software development life cycle concepts]
>*Kanban* - You know what this is
*Lean Programming* - what can we cut out that doesn't contribute to productivity
RAD - Rapid application development (do it as quickly as possible)
Agile - a series of waterfall projects

Scrum master removes impediments to development.
Product owner provides the requirements.
Devs use the product owner's requirements to work out what to do.

If it isn't trivial don't do it.
If it's not trivial, break it down until it is!
##### Day 3
Matcher functions take an expected value and actual value and make an assertion about the two values.

Simple Testing Framework:
```js
//test-framework.js

export const assertEquals = (actual, expected) => actual === expected; //! 

export const assertTrue = (actual) => actual === true;
```

Tests come in 3 parts:
- Arrange
- Act
- Assert

Testing Framework in Use:

```js
//test-file.js

// Arrange
const input1 = 10;
const input2 = 15;
const expected = input1 + input2;
let actual, result;

// Act
actual = addTwoNumbers(input1, input2); //* Call the unit under test

// Assert
result = assertEquals(actual, expected);
```

Test-driven development means:
1. You're not allowed to write any production code unless it is to make a failing unit test pass
2. You are not allowed to write any more of the unit tests than is sufficient to fail
3. You are not allowed to write any more production code than is necessary to pass one failing unit test  (e.g. if a test is looking for an output of 10, then you can hardcode 10)

>[!warning]
>Make sure your test fails before you right code for it!

``
Three Golden Rules of Programming:

1. Make it trivial
2. Laser focus on the requirements
3. Strip away the context

Pair Programming Task:

Create a user story then domain model then test then write the test then fail the test then write the production code. Do this in ping pong style.

##### Day 4

Remember Tests are written with triple A batteries!

Arrange
Act
Assert

Remember to write a test, run it to make sure it fails, then write a function that makes it pass. The test should be as limited in scope as possible, and so should the function you write for it.

We go from requirements to user stories to domain models to tests, to implementation code.

Here's a domain modelling example:
- As a member of the public, I'd like to add an item to my basket, so that I can order a bagel when I want to.

Remember the "As a..." part is not included in domain modelling.

| Objects | Properties                 | Messages       | Output |
| ------- | -------------------------- | -------------- | ------ |
| Basket  | basketItems @Array[@Items] | addItem(@Item) | @Void  |
| Item    | id @String                 |                |        |

>[!tip]
>In the Kanban, only have one piece in progress in order to avoid context switching and maintain focus, as well as to allow other team members to work on them. Break tasks down into individual non-related pieces of work.

You would test this by checking the length of the array.

Folder Organisation:
src - index.js (implementation code)
spec - basket.spec.js (tests)

Remember to npm init and add a line for type: module so that you can use modern JS imports.

Example of test for above domain model:

```js
import { assertEquals } from "../spec/test-framework/test-framework.js";
import basket from "../src/basket.js";

// AFTER EACH Function
const afterEach = () => {
    expected = undefined;
    actual = undefined;
    result = undefined;
    testItem = undefined;
    basket.basketItems = [];
};

//? TEST 1
//* Add item to basket using addItem and expect array (basketItems) has increased in length by 1

console.log(`Test 1`);
console.log(`==================`);
console.log(
    `Add item to basket using addItem and expect array (basketItems) has increased in length by 1`
);
// Arrange
let expected = basket.basketItems.length + 1;
let actual, result;
let testItem = {};

// Act
basket.addItem(testItem);
actual = basket.basketItems.length;
// Assert
result = assertEquals(actual, expected);

// Report
console.log(result ? `Pass` : `Fail`);
!result && console.log(`Expected: ${expected}; Actual: ${actual}`);
console.log(`==================`);

// Clean Up
afterEach();

//! END OF TEST 1

//? TEST 2
//* Test that item passed to addItem is actually added to the basket

console.log(`Test 2`);
console.log(`==================`);
console.log(`Test that item passed to addItem is actually added to the basket`);
// Arrange
//! testItem is defined in the test setup for test 1
expected = true;

// Act
actual = basket.basketItems.includes(testItem);

// Assert
result = assertEquals(actual, expected);

// Report
console.log(result ? `Pass` : `Fail`);
!result && console.log(`Expected: ${expected}; Actual: ${actual}`);
console.log(`==================`);

```

And here's the code that makes the test go green:

```js
const basket = {
    addItem: function (item) {
        this.basketItems.push(item);
    },
    basketItems: [],
};

export default basket;
```

>[!tip]
>Arrow functions don't change the context of `this` (it remains `window`) therefore if you declare functions inside JavaScript objects and want to refer to object properties, you should not use arrow functions.

Tests should be independent! Therefore 'clean-up' any variables before moving on to the next test.

1. Variables and functions get hoisted to the top of scope.
2. Classes don't get hoisted to the top of scope.
3. Arrow function declaration names get hoisted but not the function itself. Therefore you can reference the arrow function before it's declared but it isn't initialised until its line of declaration.
4. Let and const are hoisted but not initialised till their line.

Use the debugger with breakpoints!

Use WATCH and add the variable name (e.g. `!result`) which is part of an expression that you wish to know the value of.

##### Day 5

When doing challenges commit every test then every solution to the test. Write descriptive commit messages.

Write good documentation with headings, subheadings and bulletpoints. Use ChatGPT to help.

Make sure the requirements are met fully.

```
	setMaxCapacity(newMaxCapacity) {
		newMaxCapacity > -1 ? this.maxCapacity = newMaxCapacity : ;
  }
```


#### Week 2
##### Day 1

No classes and 6th May and 27th May

`const` locks on the stack. You can't reassign the primitive, but you can change the values inside it. For example you can create an object and assign it to a variable with const, then add a key/value pair to it. But you can't change that variable to undefined.

Javascript passes values.

##### Day 2 

in your testing framework, add `new Error` for a failing test.

```js
export const assertEquals = (actual, expected) => {
    if (actual !== expected) {
        throw new Error(`Expected ${expected}, but got ${actual}`);
    }
    return true;
};

export const it = (description, testFunc) => {
    try {
        testFunc();
        console.log("\x1b[32m%s\x1b[0m", `\t${description}`);
    } catch (error) {
        console.error("\x1b[31m%s\x1b[0m", `\t${description}`);
        console.error("\x1b[31m%s\x1b[0m", `\t\t${error.message}`);
        console.log(error.stack);
    }
};

export const xit = description => {
    console.log("\x1b[33m%s\x1b[0m", `\tTEST SKIPPED: ${description}`);
}
```

Here's how to write tests:

```js
import {
    assertEquals,
    assertTrue,
    it,
} from "./test-framework/test-framework.js";

import Wallet from "../src/Wallet.js";

it("should create a new instance of the Wallet class", () => {
    // Arrange
    // Act
    const testWallet = new Wallet();
    // Assert
    assertTrue(testWallet instanceof Wallet);
});

it("should have a balance property that is initialized to 0", () => {
    // Arrange
    const expected = 0;
    // Act
    const testWallet = new Wallet();
    // Assert
    assertEquals(testWallet.getBalance(), expected);
});

it("should be able to initialise a Wallet with a non-zero balance", () => {
    // Arrange
    const expected = 100;
    // Act
    const testWallet = new Wallet(expected);
    // Assert
    assertEquals(testWallet.getBalance(), expected);


it("should add 200 to the balance when addCash is called", () => {
    // Arrange
    const expected = 200;
    const testWallet = new Wallet();
    // Act
    testWallet.addCash(expected);

    // Assert
    assertEquals(testWallet.getBalance(), expected);
});

it("should be able to remove cash", () => {
    // Arrange
    const initialBalance = 200;
    const amtWithdrawn = 50;
    const testWallet = new Wallet(initialBalance);
    // Act
    testWallet.removeCash(amtWithdrawn);
    // Assert
    assertEquals(testWallet.getBalance(), initialBalance - amtWithdrawn);
});

it("should not be able to remove cash if amount requested is more than the cash balance", () => {
    // Arrange
    const initialBalance = 200;
    const amtRequested = 250;
    const testWallet = new Wallet(initialBalance);
    // Act
    testWallet.removeCash(amtRequested);
    // Assert
    assertEquals(testWallet.getBalance(), initialBalance);
});
```

How to pass the tests:

```js
export default class Wallet {
    // properties
    #balance;

    // constructor
    constructor(balance = 0) {
        this.#balance = balance;
    }

    // behaviours
    getBalance = () => this.#balance;

    addCash = (amt) => {
        this.#balance += amt;
    };

    removeCash = (amt) => {
        if (amt <= this.#balance) {
            this.#balance -= amt;
        }
    };
}
```

JS is not a proper OOP language. It is object-based and contains some syntactic sugar to allow for some OOP-like programming.

We're going to be using Jasmine for JS unit testing:

```js
//Add Jasmine to your package.json
npm install --save-dev jasmine

//Initialize Jasmine in your project
npx jasmine init

//Set jasmine as your test script in your package.json
"scripts": { "test": "jasmine" }

//Run your tests
npm test
```

Use `--save-dev` to specify that this is only for development purposes and won't be bundled for the end user.

Chalk npm package for coloured console logs.


Jasmine syntax:

```js
expect(testWallet).toBeInstanceOf(Wallet);
```

```js
describe("Wallet Initialisation Tests", () => {
	// put tests here... (describe function indicates a suite of tests - basically a wrapper)
}
```

`describe` let you group tests together.

You can skip suites using `xdescribe`.

Single responsibility testing: 
```js
const testCard = jasmine.createSpyObj("testCard", {
	getName: () => "Test Card",
});
```

Replace arrange with this:

```js
let testWallet, testCard;

// Will replace REPEATED arrange code
beforeEach(() => {
	testWallet = new Wallet();
	testCard = jasmine.createSpyObj("testCard", { getName: "Test Card" });
})
```

Always do this: 

```js
afterEach(() => {
	testWallet = undefined;
	testCard = undefined;
})
```

##### Day 3

Ignore Further tasks and task 5 in the second challenge.

Classes should have one job.

For example: the Card class defines the behaviour and properties of a card. A card does not give it details away. That should be the responsibility of a separate CardReader class.

See Ed's wallet-single-responsibility repo for an example.

*Professional Skills Session With Blair*
___
- Feedback is *Good*!
- Best feedback is from peers - learn to give effective feedback
- Feedback plays a pivotal role in professional development!
- It's not about denigrating someone - it's about helping that person to grow
- Promoting this growth mindset can create a much more collaborative mindset
- Constructive criticism creates *trust*
- Demonstrates a commitment to mutual growth and an environment where individuals feel secure in sharing their ideas and concerns

Good feedback:
- Specific - helps the receiver to understand better
- Timely - prompt feedback reach a fresh mind
- Balanced - acknowledge positives as well as areas from improvement
- Conscientious- be aware of sensitivities
- Constructive tone - make wise language choices that offer encouragement

Constructive criticism is a gift!

What to avoid:

| Avoid                            | Why                                                            | Alternatives                                                                         |
| -------------------------------- | -------------------------------------------------------------- | ------------------------------------------------------------------------------------ |
| Absolutes - 'Always' or 'Never'  | Sounds exaggerated, general, or unhelpful                      | "Sometimes", "Ocassionally", "Frequently"                                            |
| "But"                            | Everything before the but is ignored - creates a negative tone | "And"                                                                                |
| "You Should" or "You Must"       | No one wants to be ordered around                              | "Consider", "Try", "Might Want To Try"                                               |
| "That's Wrong" or "You're Wrong" | Bluntness can be demotivating and unproductive                 | "Have you tried...", "There might be an alternative approach"                        |
| Generic praise or criticism      | Doesn't provide actionable insights                            | Be more specific                                                                     |
| Negative labels                  | Hurtful and won't address specific areas                       | Focus on behaviors or actions: "I noticed that the project wasn't completed on time" |

Techniques to deliver feedback:

- Great job... , consider.... (actionable point of improvement) Despite (insert feedback area here) challenges, your commitment significantly contributes to the team's success.

##### Day 5

Professional Skills - Working in Teams
___
- Teamwork is collaboration to achieve a common goal.
- Pool knowledge and resources to achieve an outcome unattainable individually.
- A truly inclusive and high-performing team has a diversity of skills and a rich tapestry of experiences, life journeys, and cultural backgrounds that collectively contribute to a dynamic and innovative collaborative environment.

Benefits of diversity:
- Broad perspectives
- Enhanced creativity and innovation
- Improved decision-making
- Increased employee engagement

Diversity helps a company meet diverse customer needs:
- Cultural competence  - tot tailor products, services, or interactions
- Reflecting customer diversity - creates a more relatable and trustworthy image
- Global perspective - understanding cultural nuances and preferences of customers

How to foster inclusive teamwork...

Inclusive Language:
- Inclusive language - "Hey team", "Hello everyone" instead of "hey guys"
- Use "We" - emphasises teamwork
- Neutral job titles - "sales representative" instead of "salesman"
- Gender-neutral pronouns - "they"

Neurodiversity Inclusivity:
- There a many different ways of thinking and working - 15% of the workforce are neurodiverse
- Provide clear instructions - written instructions, visual aids, step-by-step guides etc.
- Flexible work environment.- provide quiet workspaces, flexible work hours, remote work options
- Ask for preferences - written communication or verbal discussion
- Avoid making assumptions - ask questions to understand perspectives and preferences
- Be patient and understanding - recognise that neurodivergent individuals may process information differently or require additional time to complete tasks. 
- Respect sensory needs - respect preferences regarding lighting, noise levels, *temperature* or other environmental factors that may impact neurodivergent individuals.

Successful teams excel in collaborative effort towards common goals:
- Understanding the Team's mission - the "why" behind your collective efforts. This clarity provides a shared sense of purpose.
- Defining clear goals - clearly outline specific, measurable, achievable, relevant, and time-bound (SMART) goals.
- Role clarity - define clearly each team member's role and responsibilities. Knowing who does what fosters efficiency and avoids duplication of efforts.
- Constant communication - the most important thing

Strategies for collaboration:
- Regular check-ins - discuss progress, address concerns, and recalibrate strategies. This ensures everyone is on the same page.
- Leverage individual strengths - identify and leverage the unique strengths of each team member - a diverse set of skills contributes to a more robust and effective team

#### Week 3
##### Day 1

How Java Works:
1. Java dev writes code in .java file
2. Compiler compiles .java text to bytecode
3. Bytecode is run on the JVM (Java Virtual Machine - OS specific)
4. JVM starts execution from Public static void main

JDK contains JRE which contains JVM.
(Dev kit, runtime environment, virtual machine)

##### Day 2

Domain models in the beginning can be incomplete. You can go back and modify them if you've had to create extra properties or methods. The models are just a tool to help at the beginning of coding.

This is how to cast a type:

```java
engineer.setAge((byte) 22)
```

This is useful when having an age property as you can use a memory-efficient `byte` primitive type. Java assumes that all integer values are of type `int` unless specified through casting. Floating type numbers (decimal-containing values) are assumed to be a `double`.

A useful way to convert numbers to strings is to concatenate them into a string

```java
int number = 10
String string = "" + number
```

Static properties are accessible through the class **as well as its instances** (not the case in JS), but its value is consistent throughout.

```java
// nextID is a static property
Engineer engineer = new Engineer()
Engineer.nextID == engineer.nextID
// returns true
```

You can use static properties like this to set instance properties based on the state of the class.

```java
class Enginer{
private static int nextID = 0
//...

	Engineer() {
		this.id = nextID++ 
	}
}
```

`nextID++` returns the current value of `nextID`, then increases the value of `nextID` by 1.
(pre-increment operator)

`++nextID` would increment first before returning the value of `nextID`.
(post-increment operator)

This also applies for `--`.

How to print an array:

```java
Arrays.toString(array)
```

You can't just `sout` an array as it's a reference.

The compiler can only check types - hence it outputs errors when necessary. Exceptions occur only at runtime, which are related to values - whether the value exists in memory etc.

Enhanced Java For Loop (For-Each):

```java
for(String name : names) {
///...
}

// basically forEach of names array
```

You can chain constructors

```java
public Trainer(String name, int age) {
		this.name = name
		this.age = age
}

public Trainer(String name, int age, String gender) {
	this(name, age)
	this.gender = gender
}
```

The bottom constructor is calling the top constructor using the `this` keyword.

This is because the top constructor matches the parameters given to the `this` function.

##### Day 5 

When initialising an array in Java, you need to specify the length and the type.

Arrays reserve space for the number of elements specified, which could mean wasted RAM if the programme doesn't use all of the alloted slots.

Use single quotes for a `char` (primitive) or `Character` (class).

Use `10.29d` or `56.48f` for floats and doubles.

`Arraylist` only takes a wrapper class as a type, no primitives allowed.

`Arraylist` when adding an `int` for example to it, it will implicitly convert it to an instance of the `Integer` class. 

Getting an index that doesn't exist will give a runtime exception.

Get to know the ins and outs of `ArrayList` methods.

How to add JUnit
___
1. Go to `pom.xml` and right click then `Generate -> Dependency`
2. Type `junit-jupiter`
3. Choose `org.junit.jupiter:junit-jupiter:5.XX.XX` (latest version)
4. Click Add
5. Save the `pom.xml` file
6. Right click the project and select `Maven => Update Project...`

Add code coverage
___
Codementor website jacoco installation tutorial

How to use JUnit
___
```java
package com.digitalfuturesacademy.app;

import org.junit.jupiter.api.Test

import static org.junit.jupiter.api.Assertions.asserTequals

public class EngineerTest {
	// Arrange

	// Act

	// Assert
}
```

Run Jacoco through Maven.

You can check the `index.html` file in the Maven folder for analysis of test coverage.

Exceptions
___

Explore the exceptions.

Useful exceptions:

`IllegalArgumentException` - for example: "Argument cannot be empty"

Use these exceptions in your production code!

You can test exceptions with `assertThrows()` in JUnit.

For exceptions you should use `try...catch` blocks otherwise an exception would exit the programme.

```java
try {
//...
} catch (IllegalArgumentException e){
	System.out.println(e.getMessage())
	System.out.println("Please re-enter details")
} catch (RuntimeException e){
	System.out.println(e.getMessage())
	System.out.println("Please re-enter details")
} finally {
	// Always run
	System.out.println("Details used to create user")
}
```

This way the error is not returned, but rather the error object's message is printed out as a string.

Go from most specific version of the exception from the top to the least specific at the bottom.

##### Day 4

Set SMART Goals - Specific, Measurable, Achievable, Relevant, TIme-bound

Companies are looking for growth-oriented talent. People who view mistakes as learning opportunities, and who have endurance with regards to their goals.

There's a reflection diary in Noodle. After each module you need to reflect on your strengths and areas of improvement. Upon reflection you will create SMART goals to accomplish by the following module.

Construction and execution of these goals are directly linked to your personal development and progress within the academy.

Graduates did 40% better than non-graduates in setting and achieving their SMART goals.

##### Day 5

Only double quotes for `String` class.

You can't change a `String` variable as it is immutable. (The reference is immutable).

Java has a special memory area called the String Pool. This is an area of the heap. 

https://www.baeldung.com/java-string-pool

`new String()` creates an object that refers to a memory location in the string pool (this is where the strings are actually stored). Simply assigning a value to a `String` (e.g. `String name = "Araz")` just stores a reference to the memory location in the string pool rather than an object on the heap (these are called interned strings).

String Equality/Comparison
___
Use `name.equals(name2)` to check if the sequence of characters in both strings are the same. This does not check to see if they are the same object.

Concatenation
___
```java
name.concat(name)
```

Do this instead of using `+`.

Formatting
___
```java
String.format("%s", "hello")
```

"hello" will replace percentage sign s.

Newline
___
```java
"%n"
```

Decimal Integer
___
```java
"%d"
```


Byte Roll Over
___
For a byte, when you add to 1 to 127, then it just rolls over to -128. 

Implicit Conversion of Calculations
___
All calculations are implicitly converted to int. If you want to add 2 bytes then you have to cast the result of the calculation:

```java
byte newByte = (byte) (byte1 + byte1)
```

Look into:
nextInt
nextDouble

#### Week 4

##### Day 1/2 (Bank Holiday)

Use mermaid.js for class diagrams.

$ indicates a static variable.

Use Mockito:

```java
Engineer engineer mock(Engineer.class)
```

Use Jacoco for reports (test coverage).


Use errors!

```java
IllegalArgumentException
RuntimeException
```

Private methods are not directly unit tested! A test can't access them.

They are tested when you test a public method that calls them.

Separate out validations for methods into separate static methods.

Use Java Lambda functions.

##### Day 3

Try `try` with 

Check out Mockito's mock static.

Watch a mockito tutorial.

Don't commit a failing test. Write the test, pass it, then add and commit.

Varargs:

```java
public void myFunction(int... numbers) {
    for (int num : numbers) {
        System.out.println(num);
    }
}
```

Variable number of arguments for a method.

_Polymorphism_

Preference to widening conversions (e.g. `byte -> int`) then auto-boxing (e.g. `int -> Integer`) then varargs (`int...`).

```java
processData((byte)10)

void processData(int... value) {}
void processData(Integer value) {}
```

It would choose `Integer`.

Exact Match
Promotion (primitive)
Autoboxing
Varargs
Widening
Overloading resolution with subtypes (specificity)
Ambiguous error

Decimals are automatically `double` NOT `float`.

Math.round() takes a float or double then returns closest long or int.

Math.ceil()/Math.floor() returns a double.

Adding a variable to a string calls the toString() method of the object referred to by the reference in that variable.

If no toString() override then it just prints memory location.

Unary/Binary operators promote primitives but NOT Strings as they are psuedoprimitives.

##### Day 4

How to pass the interview:

Passion for tech
Curiosity
Growth mindset
Proactivity

Passion for tech - 
Constantly changing, lots of collaboration, wanting a long-term career in tech, personal interest in tech.

Curiosity - 
How do you learn something, how have you learned something in the past, ask questions that show curiosity.

Growth mindset -
How do you embrace challenges or chip away at overarching goals. It's okay to talk about things that go wrong as long as you follow up with what you've learned. Say what you've learned with confidence. Don't blame others. Bring a notebook to the interview.

Proactivity - 
Give an example of a time when you were one step ahead or took the initiative on a task. Use anecdotes.

_Introduction_
- Set up with a 'Thanks for the opportunity'. 
- Who I am now - what did you accomplish at the academy 10 secs
- How did I get here - what did you do in the past, how did you enter the world of technology 10 secs
- What do you aspire to do - this should be personal to you 1 sentence
- What are your interests - this separates you from the crowd 1 sentence

Keep it brief.

First of all thank you for the opportunity. 

I've just completed the Digital Futures academy where I attained a Java Foundations Certificate and learned to create full-stack web applications using the MERN stack. 

I'm coming to tech from accounts where I worked for two years after graduating from the University of Leeds in Economics and Philosophy. My curiosity about tech and the inner workings of the internet led me to take an online course in web development and I subsequently spent a lot of my spare time working on building my programming knowledge.

My aspiration is to work 

I have a number of creative interests including photography.

_Company Alignment_
- Mission
- Goals
- Culture

Align your goals, aspirations and experiences with these points.

Reference facts and figures from news articles.

Check out their social media, any news articles on them, their blogs.

You won't get the job if you don't ask any questions at the end of the interview.

Have 4-5 questions prepared - bring your notebook.

Set up the question then ask an open-ended one.
