```table-of-contents
```
# 1. Getting Started

## What is automated testing?
Writing code in order to test some other code, and then running those tests in an automated fashion.

## What are the benefits of automated testing?
- The alternative is manual testing, which is time consuming and tedious. Manual testing duration increases exponentially with increased application complexity. 
- **Save time**- automated tests are repeatable. You can run the same tests on hundreds or thousands of functions within moments. 
- **Deploy with confidence** - catch bugs before deployment.
- **Refactor with the confidence** - ensure that functionality has not changed.
- **Improved method quality** - catch edge-cases and improve the scope of your functions

## What types of tests are there?
- **Unit Testing:** 
	- Tests a class *or* multiple classes without its/their external dependencies (use *mocks* where necessary)
	- Pros -
		- **Cheap to write**
		- **Executes fast**
	- Cons -
		- **Lacks scope** - doesn't give a lot of confidence in the reliability of your application
- **Integration Testing:**  
	- Tests a class or classes with its/their external dependencies (e.g. files, databases, libraries)
	- Pros -
		- **Better scope** - gives more confidence in the health of you application
	- Cons -
		- **Slower** - unit tests are much faster to run

>[!warning]
>An integration test is ***not*** simply a matter of testing two classes together.
>
>This definition of integration testing assumes that it is taking a few classes or units of code and testing their behaviour together. 
>
>This is a sure-fire way to get a fragile test-suite that is tied to you implementation details, and it will fall apart as those implementation details change. This will slow you down. Avoid this pitfall.

- **End-to-end testing:**  
	- Drives an application through its user interface
	- Tools - 
		- **Selenium** - records user interaction with the application and checks if the application gives the right result or not.
	- Pros - 
		- **Greatest confidence** - this testing simulates a user's experience and 
	- Cons - 
		- **Very slow** - requires launching the application and testing it through the UI (go through login, navigate pages, fill out forms, inspect the result)
		- **Very brittle** - a small enhancement or change in the user interface can easily break these tests

## What type of tests should I use - Test Pyramid)?

![[Pasted image 20241127233010.png]]

**The Test Pyramid:**
- **Most** of your tests should be **unit tests**
- **Followed** by **integration tests**
- Finally **very few end-to-end tests** for the key functions of the application.

**End-to-end tests should:**
- Only test happy paths
- Leave edge-cases to unit testing

However, this is just a guideline. The actual ratio depends on the project.

Unit testing is good for quickly testing the logic like conditional statements and loops (use this for complex logic)

But not every application has functions with complex logic.

If your app is more of a CRUD app, use more integration tests than unit tests.

**Key Takeaways:**
- Favour unit tests over e2e tests because they are the fastest to run, cheapest to write, and very precise. They give rapid feedback
- Cover unit tests gap with integration tests.
- Use end-to-end tests sparingly.

## What is the tooling?

**C# testing libraries:**
- NUnit - one of the earliest
- MSTest - built into Visual Studio
- xUnit - gaining a following

The framework contains:
- **Utility library** - methods you use to write tests
- **Test runner** - runs your tests and gives you a report on the pass/fail status of your tests

>[!tip]
>Focus on the fundamentals - not the tooling!

The course will start with MSTest, then move on to NUnit.

## Writing your first unit test

- Create a Unit Test Project in your solution
- Name it with the name of the actual project itself + .UnitTests (e.g. TestNinja.UnitTests)
- We are separating unit and integration tests are they will be run more frequently due to their speed.
- MSUnit uses decorators to declare a test suite (`TestClass`) and a test method (`TestMethod`). Decorators, referred to as ***attributes*** in C# are declared using square brackets.

```c#
namespace TestNinja.UnitTests
{
    [TestClass]
    public class UnitTest1
    {
        [TestMethod]
        public void TestMethod1()
        {
        }
    }
}
```

### Convention for naming and organising test methods

Each test method name should contain three pieces of information
1. Method being tested
2. Scenario (which path/what arguments passed to method)
3. Expected behaviour (what does the method return given the argument/s)

```c#
public void CanBeCancelledBy_Scenario_ExpectedBehaviour()
{
}
```

There is also a convention for the test block itself known as Triple A:

- Arrange - initialise your object/s
- Act - Call the method you want to test
- Assert - 

```c#
public void CanBeCancelledBy_UserIsAdmin_ReturnsTrue()  
{  
// Arrange
// Act
// Assert
}
```

### First Test Example

**Method being tested:**

```c#
 public bool CanBeCancelledBy(User user)
{
	return (user.IsAdmin || MadeBy == user);
}
```

**The test:**
```c#
public void CanBeCancelledBy_UserIsAdmin_ReturnsTrue()  
{  
    // Arrange  
    var reservation = new Reservation();  
    var user = new User { IsAdmin = true };  
  
    // Act  
    var result = reservation.CanBeCancelledBy(user);  
  
    // Assert  
    Assert.IsTrue(result);  
}
```

## Testing All The reservation Tests

By convention, the test class file should be the same as the tested class + Tests (e.g. `Reservation.cs` should be tested by `ReservationTests..cs`)

Here is the complete class containing all three paths for the Reservation class:

```c#
// ReservationTests.cs
using TestNinja.Fundamentals;

namespace TestNinja.UnitTests;

[TestClass]
public sealed class ReservationTests
{
    [TestMethod]
    public void CanBeCancelledBy_UserIsAdmin_ReturnsTrue()
    {
        // Arrange
        var reservation = new Reservation();
        var user = new User { IsAdmin = true };

        // Act
        var result = reservation.CanBeCancelledBy(user);

        // Assert
        Assert.IsTrue(result);
    }

    [TestMethod]
    public void canBeCancelledBy_SameUserCancellingReservation_ReturnsTrue()
    {
        // Arrange
        var user = new User();
        var reservation = new Reservation {MadeBy = user};
        
        // Act
        var result = reservation.CanBeCancelledBy(user);

        // Assert
        Assert.IsTrue(result);
    }

    [TestMethod]
    public void cannotBeCancelledBy_NotAnAdminOrMadeByUser_ReturnsFalse()
    {
        // Arrange
        var user = new User();
        var reservation = new Reservation {MadeBy = new User()};

        // Act
        var result = reservation.CanBeCancelledBy(new User());

        // Assert
        Assert.IsFalse(result);
    }
}
```

>[!note]
>It is very useful to use **object initializer syntax** as it allows you to set an objects properties or fields directly at the time of creation without the need for separate property assignments.
>
>This syntax is particularly common in C# programming for scenarios like configuring options, setting up test data, or populating objects in a fluent, declarative style.
>
>```csharp
>var reservation = new Reservation();
reservation.MadeBy = new User();

## Refactoring with confidence

Tests act as a sort of documentation for the source code due to the naming conventions.

Therefore you can use tests when refactoring to better understand the purpose of the original code, and then 

## Using NUnit

Install:
- NUnit
- NUnit3TestAdapter (not needed with Rider/Resharper)

Differences with NUnit when coming from MSTest
- `[TestFixture]` attribute instead of TestClass
- `[Test]` attribute instead of TestMethod
- `Assert.That(result, Is.True)` - English-like syntax
- `Assert.That(result == true)` - An alternative syntax that also works

>[!tip]
>Rider is very particular so I recommend creating an NUnit project from scratch (which gives multiple test library options) rather than trying to change the `.cproj` file in order to migrate from one testing library to another.

```c#
namespace TestNinja.UnitTests;

public class Tests
{
    [Test]
    public void CanBeCancelledBy_UserIsAdmin_ReturnsTrue()
    {
        // Arrange
        var reservation = new Reservation();
        var user = new User { IsAdmin = true };

        // Act
        var result = reservation.CanBeCancelledBy(user);

        // Assert
        Assert.That(result, Is.True);
    }
    
    [Test]
    public void canBeCancelledBy_SameUserCancellingReservation_ReturnsTrue()
    {
        // Arrange
        var user = new User();
        var reservation = new Reservation {MadeBy = user};
        
        // Act
        var result = reservation.CanBeCancelledBy(user);

        // Assert
        Assert.That(result, Is.True);
    }
    
    [Test]
    public void cannotBeCancelledBy_NotAnAdminOrMadeByUser_ReturnsFalse()
    {
        // Arrange
        var user = new User();
        var reservation = new Reservation {MadeBy = new User()};

        // Act
        var result = reservation.CanBeCancelledBy(new User());

        // Assert
        Assert.That(result, Is.False);
    }
}
```

## What is Test-Driven Development?

You write tests ***before*** writing production code.

**Foundations of TDD:**
Repeat these tests over and over until you have written a complete feature -
1. Start by writing a failing test.
2. Write the simplest code to make the test pass.
3. Refactor if necessary

**Benefits of TDD:**
- **Testable source code **- no changes needed to make you code testable
- **Full coverage** - all you code will be tested
- **Simpler implementation** - only write enough code to pass the tests

>[!note]
>Remember that you should be using TDD wherever it's beneficial to your project. It is not the be-all-end-all of software development.

# 2. Fundamentals of Unit Testing

**What we'll be learning in this chapter:**
- Characteristics of good unit tests
- What to test and what not to test
- Naming and organising tests
- Basic techniques
- Writing reliable tests
## Characteristics of good unit tests

**Good unit tests are:**
- **First-class citizens** - they're as important or even more important that your production code
- **Clean, readable, and maintainable** - you don't want to spend unnecessary time debugging unit tests
- **No Logic** - No conditional statements or loops (because you might make a mistake, and later on think that the mistake is in your production code instead)
- **Isolate** - tests should not call each other or rely on state in another test
- **Not too specific/too general** - too general and it won't give you any confidence in your code; too specific and you're wasting time

## What to test and what not to test

**Make sure your production code is separated into digestible chunks:**
- Sometimes the problem is that your production code is too complex or long.
- Clean code and unit testing go hand-in-hand.
- Your unit tests should be testing units (i.e. digestible chunks of code)

**What should we test?**
- This depends on whether a function is a ***query*** or a ***command***.
	- **Query** (returns an item of interest);
		- Test the outcome (return) of a method
		- Test all execution paths of a function
	- **Command** (performs an action and sometimes returns a result- changes the state of an object, writing to a database, calling a web service, sending a message to a message queue)
		- Test the outcome of a method - sometimes this is a return, sometimes it is just making a call to a particular dependency
		- Use your judgement to decide what the outcome is and what should be tested

**What not to test:**
- Never test **language features** - don't make sure that C# is working correctly (e.g. test that a class's properties are working etc.)
- **3rd party code** - don't test a libraries methods

## Naming and organising tests

- **Name the test class by the class being tested** -  TestNinja => TestNinja.UnitTests
- **Separate unit and integration tests** - we run unit tests a lot but integration tests just before pushing
- **Cover all paths of execution** - the number of tests should be equal or greater than the number of execution paths.
- **Use the test naming convention** - the name of your tests should clearly specify the business rule you're testing.
- **Group tests for one method into a single class** - Sometimes a method requires many tests, in that case it may be worth giving it its own test class

**Good test names (by convention):**

`[MethodName]_[Scenario]_[ExpectedBehaviour]`

## Introducing Rider

Rider is an IDE developed by JetBrains, the brains behind ReSharper (and IntelliJ IDEA).

Rider/Resharper gives you the option of running a single test or nested test class by using an icon to the left of the method/class declaration.

## Writing a simple unit test

If a method only has a single execution path, you should only use one test for it.

**Example:**

```c#
namespace TestNinja;

using System.Collections.Generic;

public class Math
{
    public int Add(int a, int b)
    { 
        return a + b;
    }
        
    public int Max(int a, int b)
    {
        return (a > b) ? a : b;
    }

    public IEnumerable<int> GetOddNumbers(int limit)
    {
        for (var i = 0; i <= limit; i++)
            if (i % 2 != 0)
                yield return i; 
    }
}
```

Use a very simple test to check that method returns the intended result:

```c#
namespace TestNinja.UnitTests;

[TestFixture]
public class MathTests
{
    [Test]
    public void Add_WhenCalled_ReturnsSumofArguments()
    {
        // Arrange
        var mathObject = new Math();
        
        // Act
        var sum = mathObject.Add(5, 10);
        
        // Assert
        Assert.That(sum, Is.EqualTo(15));
    }
}
```

## Black-box testing

The `Max` method of the `Math` class in the previous section has two execution paths. If a is bigger we return a, and if b is bigger we return b.

It's good to write down all of the test declarations for a method so that you don't forget a path. This is particularly useful if there are numerous paths.

```c#
[Test]
public void Max_WhenFirstArgumentIsGreater_ReturnsFirstArgument() {}   

[Test]
public void Max_WhenSecondArgumentIsGreater_ReturnsSecondArgument() {}  

[Test]
public void Max_ArgumentsAreEqual_ReturnsSameArgument() {}  
```

>[!important]
>You should not be relying on a particular implementation for your method for a test to work. Think of the method as a black box.

## Set-up and tear down

It is possible to initialise variables common to multiple tests using a single line of code. This is done whilst maintaining the separation of state between tests.

In the following example, each test initialises the `mathObject` from scratch.

```c#
    [Test]
    public void Max_WhenFirstArgumentIsGreater_ReturnsFirstArgument()
    {
        // Arrange
        var mathObject = new Math();
        
        // Act
        var max = mathObject.Max(10, 0);
        
        // Assert
        Assert.That(max, Is.EqualTo(10));
    }    
    
    [Test]
    public void Max_WhenSecondArgumentIsGreater_ReturnsSecondArgument()
    {
        // Arrange
        var mathObject = new Math();
        
        // Act
        var max = mathObject.Max(0, 10);
        
        // Assert
        Assert.That(max, Is.EqualTo(10));
    }    
    
    [Test]
    public void Max_ArgumentsAreEqual_ReturnsSameArgument()
    {
        // Arrange
        var mathObject = new Math();
        
        // Act
        var max = mathObject.Max(2, 2);
        
        // Assert
        Assert.That(max, Is.EqualTo(2));
    }
```

We can avoid this repetition of code through attributes:
- `[SetUp]` - Runs some code at the start of tests
- `[TearDown]` - Runs some code at the end of tests

To use these attributes, simple add the variables you'll need to the top of your class code as private fields, and reference them in a setup method decorated with an attribute:

```c#
public class MathTests
	private Math _math;
	
	[SetUp]
	public void SetUp()
	{
		_math = new Math();_
	}
...
}
```

The method declarations under the attribute line are named SetUp and TearDown ***by convention*** (not rule).

This allows you to remove the arrange part of your test method code:

```c#
    [Test]
    public void Add_WhenCalled_ReturnsSumofArguments()
    {
        // Arrange
        
        // Act
        var sum = _math.Add(5, 10);
        
        // Assert
        Assert.That(sum, Is.EqualTo(15));
    }
```

## Parameterised tests

NUnit has a concept known as parameterised tests. It allows you to pass values as arguments into the test method instead of hardcoding them.

Use the `[TestCase()`] attribute to pass values to the test methods. This allows you to reduce your number of tests are they are made redundant by the fact that you can pass multiple values reflecting different paths to a single test:

```c#
    [Test]
    [TestCase(2, 1, 2)]
    [TestCase(1,2,2)]
    [TestCase(1,1,1)]
    public void Max_WhenCalled_ReturnsTheGreaterArgument(int a, int b, int expectedResult)
    {
        var max = _math.Max(a, b);
        
        Assert.That(max, Is.EqualTo(expectedResult));
    }    
```

>[!warning]
>This is an NUnit feature - it doesn't exist in MSTest.

## Ignoring tests

**The `[Ignore("This tests is being ignored")]` attribute**:
- Add it above a test method declaration
- You can give it a string to display in the test runner

```c#
[Test]
[TestCase(2, 1, 2)]
[TestCase(1,2,2)]
[TestCase(1,1,1)]
[Ignore("This tests is being ignored")]
public void Max_WhenCalled_ReturnsTheGreaterArgument(int a, int b, int expectedResult)
{
	var max = _math.Max(a, b);
	
	Assert.That(max, Is.EqualTo(expectedResult));
}  
```
## Writing trustworthy tests

**Trustworthy tests:**
- **Can be relied upon** - if it passes, your code works, if it fails, your code needs fixing
- Can be written using:
	- **TDD** (can be complex in real world scenarios) - write a failing test and then write the simplest code to pass the test
	- **Code first** - risky because you might test the wrong thing (test might pass when it shouldn't)
- There is a technique to ensure trustworthy tests using the **code first** approach:
	- Return the wrong result in your production code deliberately
	- Your test might be implemented wrong, so simply create a bug in your production code
	- If your test still passes then the test is implement incorrectly
	- Basically - always **fail the test deliberately**
	- This applies whether the test is written first or last. If last then go back to your production code and change something related to the test's expectation, and make sure that the test fails.

>[!summary]
>Make sure it is possible to make your test fail!

## Developers who don't test their code

**The cost of a software bug:**
- Found in requirements gathering = $100
- Found in QA testing = $1,500
- Found in production = $10,000

**Don't be dogmatic:**
- You should have a mix of automated and manual testing.
- Don't test for the sake of it, use testing to enhance the development process.
- Take the middle ground.
- Be pragmatic.

**The sensitivity problem**
- Testing is like double-entry bookkeeping.
- Every piece of logic in your application code needs a test that verifies that the logic is implemented properly.

**Focus on delivering quality:**
- If a pilot can delay a flight, you can delay your push to prod.
- Spend less time fixing bugs.
- Pay the cost up-front by writing tests instead of paying a much greater cost when the bugs appear further along down the line.

# 3. Core Unit Testing Techniques

**This section will focus on:**
- Testing methods that return a value
- Testing void methods
- Testing methods that throw an exception
- Testing methods that raise an event
- Testing private methods

## Testing strings

Good unit tests shouldn't be too specific or general. Striking that balance is up to you. Aim to test exactly what needs to be tested and no more or less.

```c#
// Class being tested
namespace TestNinja.Fundamentals  
{  
    public class HtmlFormatter  
    {  
        public string FormatAsBold(string content)  
        {            return $"<strong>{content}</strong>";  
        }    
    }
}

// Test assertions (without boilerplate)

// Specific assertion - a good test in this case
Assert.That(result, Is.EqualTo("<strong>abc</strong>");

// More general asserion - bad because the method method might not contain the content given as an argument to the method
Assert.That(result, Does.StartWith("<strong>");
Assert.That(result, Does.EndWith("<strong>");
Assert.That(result, Does.Contain("abc"); // Imagine the test has been passed "abc"
```

### The IgnoreCase method

You can use this method to create assertions that ignore case:

```c#
Assert.That(result, Does.Contain("abc").IgnoreCase();
```

## Testing collections

**We will be testing this method:**

```c#
public IEnumerable<int> GetOddNumbers(int limit)
    {
        for (var i = 0; i <= limit; i++)
            if (i % 2 != 0)
                yield return i; 
    }
```

When dealing with arrays, try to give arguments to methods that return at least a few (maybe 3) elements in the collection.

Example assertions:

```c#
// Check that a collection is not empty
Assert.That(result, Is.Not.Empty);  

// Check the number of elements 
Assert.That(result.Count(), Is.EqualTo(3));  

// Check that a particular element value exists
Assert.That(result, Does.Contain(1));  
Assert.That(result, Does.Contain(3));  
Assert.That(result, Does.Contain(5));

// Best way to test the above method - tests whether the returned collection (result) contains the same elements as the collection supplied as the argument to EquivalentTo
Assert.That(result, Is.EquivalentTo(new [] {1, 3, 5}));

// Other useful assertions

// Check that the collection is sorted/ordered
Assert.That(result, Is.Ordered);  

// Check whether all the elements are different (unique)
Assert.That(result, Is.Unique);
```

## Testing the return type of methods

**In this section we'll be testing this method:**

```c#
public ActionResult GetCustomer(int id)
{
	if (id == 0)
		return new NotFound();

	return new Ok();
}
```

Testing this requires us to check the return type returned by the method when:
1. id is 0
2. Any other int is passed as id

**Two ways to check the type of a return:**

```c#
// Must be exactly the type (NotFound)
Assert.That(result, Is.TypeOf<NotFound>());  

// Expects the type or a derivative (i.e. a child type of NotFound)
Assert.That(result, Is.InstanceOf<NotFound>());
```


## Testing void methods

So far we've only looked at testing methods that return values. Now we'll turn our attention to void methods - which are inherently 'command' rather than 'query' functions. This means they interact with state in some way rather than returning a value.

**Check properties of objects:**

```c#
Assert.That(logger, Has.Property("LastError").EqualTo("a"));
```

I'll leave the full code for reference below.

**Method to test:**

```c#
namespace TestNinja.Fundamentals;

public class ErrorLogger
{
    public string LastError { get; set; }

    public event EventHandler<Guid> ErrorLogged; 
        
    public void Log(string error)
    {
        if (String.IsNullOrWhiteSpace(error))
            throw new ArgumentNullException();
                
        LastError = error; 
            
        // Write the log to a storage
        // ...

        ErrorLogged?.Invoke(this, Guid.NewGuid());
    }
}
```

**Test:**

```c#
using TestNinja.Fundamentals;
namespace TestNinja.UnitTests;

[TestFixture]
public class ErrorLoggerTests
{
    [Test]
    public void Log_WhenCalled_SetLastErrorProperty()
    {
        var logger = new ErrorLogger();

        logger.Log("a");
        
        Assert.That(logger, Has.Property("LastError").EqualTo("a"));
    }
}
```

>[!tip]
>Remember to go back to the production code and deliberately make it an incorrect implementation by commenting a line out or changing a method. Do this to ensure that your test is trustworthy and not a phoney.

## Testing methods that throw an exception

**The `Assert.Throws` methods:**

```c#
Throws.Exception
Throws.ArgumentNullException
Throws.Exception.TypeOf<ArgumentNullException>()
```

The previous section contains a `Log` method that throws an exception in three scenarios: 
1. null
2. "" (empty string)
3. " " (string with whitespace only)

We can use a parameterised test to simplify the test code. below the previous test for the method (see the example above), add the [TestCase()] attributes:

```c#
[TestCase(null)]   // There seems to be an issue with passing null
[TestCase("")]  
[TestCase(" ")]
```

Next, we need to use **delegates**;

```c#
public void Log_InvalidError_ThrowsArgumentNullException(string error)  
{  
    var logger = new ErrorLogger();  
        logger.Log(error);  
    Assert.That(() => logger.Log(error), Throws.ArgumentNullException);  

// Or use the below
    Assert.That(() => logger.Log(error), Throws.Exception.TypeOf<DivideByZeroException>()); 
}
```

>[!info]
>The lambda expression is a shorthand to define a delegate that encapsulates the Log method call. It is passed the the `Assert.That `static method as an argument. A delegate refers to a type that represents a reference to a method, allowing for methods to be passed as parameters, stored in variables, and invoked dynamically.

## Testing methods that raise an event

To check whether an event is raised, you need to subscribe to that event.

**Subscribe to an event:**
```c#
logger.ErrorLogged += (sender, args) => {}
```

**Full class code:**
```c#
namespace TestNinja.Fundamentals;

public class ErrorLogger
{
    public string LastError { get; set; }

    public event EventHandler<Guid> ErrorLogged; 
        
    public void Log(string error)
    {
        if (String.IsNullOrWhiteSpace(error))
            throw new ArgumentNullException();
                
        LastError = error; 
            
        // Write the log to a storage
        // ...

        ErrorLogged?.Invoke(this, Guid.NewGuid());
    }
}
```

**Test code:**
```c#
[Test]
public void Log_ValidError_RaiseErrorLogEvent()
{
	var logger = new ErrorLogger();
	var id = Guid.Empty;
	logger.ErrorLogged += (sender, args) =>
	{
		id = args;
	};
	logger.Log();
	
	Assert.That(id, Is.Not.EqualTo(Guid.Empty));
}
```

>[!explanation]
>An event is a mechanism for implementing the *observer pattern* in C#. It allows a class to notify other classes or components when something interesting happens.
>
>Events are typically declared using the `event` keyword, and they are often used in combination with delegates.
>
>In the test method above, `ErrorLogged` is an event being subscribed to. The lambda expression is an event handler, which will allow you to extract data from the event in the code block.

## Don't test private/protected methods

You shouldn't! They are implementation details - inside a black box. They can change easily as you refactor and restructure your work.

There are tools for testing private/protected methods, but ideally you want to just test the methods that call private/protected methods.

If you want to test a private/protected method without special tools, you'd need to change the method to a `public` one.

The problem arises when you change the implementation details of you private/protected methods. Remember, the point of these methods is to be used by other methods in the class/child classes. So if you change how they are being used, then the tests also need to change. So just save yourself the hassle and test public methods, indirectly testing the private/protected ones in the process.

>[!summary]
>- Only test the public API (public members/methods)!
>
>- If you have many large private methods in a class, it might be irksome to test the public methods of the class due to the sheer quantity of execution paths. This is a sign of smelly code. Put the private methods in a separate class and make them public.

## Code coverage

**How do we know if we have enough tests for a method or class:**
- **Code coverage tools** - Rider comes with **DotCover**. Just click the Tests button on the top toolbar and select one of the Cover options (e.g. Cover All Tests From Solution).

## Testing in the real world

... Add notes for this section.

# 5. Testing Methods with External Dependencies

## Introduction

**How to unit test tightly coupled code:**
- **Fake/test doubles** - replace production objects with a fake
## Loosely-coupled and testable code

Most legacy applications are built without unit testing in mind. You'll need to refactor them towards a testable and loosely-coupled design. Do this so that it becomes easy to replace external objects with test doubles.

**Achieving a testable and loosely-coupled design:**
1. Put the code that talks to an external resource in a separate class.
2. Extract an interface from that class - a contract that declares a bunch of methods and properties that can be used to construct a double
3. Make the class use the interface in its implementation details rather than concrete objects

**In practical terms:**
- Delete the lines with `new` keyword inside the class (creating new objects in a method would mean very tightly-coupled code, and you wouldn't be able to replace that object with a fake).
- Pass objects into a method as an argument (**dependency injection**)

>[!info]
>**Dependency injection** is key to writing good code. It makes your code loosely-coupled and thus easier to refactor, understand, and test.
>
>**What is dependency injection?**
>
>It's when you pass objects into methods (instead of constructing them directly in the method code).

## Refactoring towards a loosely-coupled design

You can use context actions (`CMD + .`) to get to the refactor menu (`control + shift + R`) where you can extract an interface from a class (i.e. create an interface with the same name, properties, and methods as the class).

# Further Questions

## What is an attribute/decorator and how is it implemented?

### What is an attribute?

An attribute is a form of metadata attached to programme elements (e.g. as classes, methods, properties etc.).

Attributes are processed by the compiler or accessed at runtime using `reflection`.

The various purposes of attributes include:
- Informing the compiler about specific rules or optimisations.
- Providing information for runtime behaviour (e.g. `Serializable`, `DataContract`).
- Customising libraries or frameworks (e.g., ASP.NET attributes like `[HttpGet]`).

### How are attributes implemented?

You can define a custom attribute by creating a class that inherits from `System.Attribute`:

```c#
public class MyCustomAttribute : Attribute
{
    public string Info { get; }
    public MyCustomAttribute(string info)
    {
        Info = info;
    }
}
```

The class defines properties or fields to store the metadata past when the attribute is applied.

#### 1. **Defining a Custom Attribute**

You create a custom attribute by subclassing `System.Attribute`.

- Add fields or properties for the metadata you want to store.
- Use constructors to define how values are passed when the attribute is applied.

Example:

```c#
using System;

[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, AllowMultiple = true)]
public class MyCustomAttribute : Attribute
{
    public string Description { get; }
    public int Priority { get; set; }

    public MyCustomAttribute(string description)
    {
        Description = description;
    }
}
```

- `AttributeUsage` defines where the attribute can be applied (`Class`, `Method`, etc.) and whether multiple instances can be used (`AllowMultiple = true`)
- The constrictor specifies the required parameters (`Description`), while properties like `Priority` can be optional.

#### 2. **Applying an Attribute**

To use your custom attribute, you place it above the target element in square brackets.

```c#
[MyCustomAttribute("This is a test attribute", Priority = 1)]
public class MyClass
{
    [MyCustomAttribute("Applied to a method", Priority = 2)]
    public void MyMethod()
    {
        // Method logic
    }
}
```

#### 3. **Accessing Attributes at Runtime**

Attributes can be accessed using **reflection**. Reflection allows the program to inspect its metadata and retrieve the attributes applied.

```c#
using System;
using System.Reflection;

class Program
{
    static void Main()
    {
        // Get the type of the class
        Type type = typeof(MyClass);

        // Fetch all attributes applied to the class
        object[] attributes = type.GetCustomAttributes(false);

        foreach (var attribute in attributes)
        {
            if (attribute is MyCustomAttribute customAttr)
            {
                Console.WriteLine($"Description: {customAttr.Description}, Priority: {customAttr.Priority}");
            }
        }
    }
}

// Output: Description: This is a test attribute, Priority: 1
```

#### AttributeUsage Explained

The `AttributeUsage` attribute configures how a custom attribute can be applied:

- `AttributeTargets`: Specifies the elements the attribute can be applied to (e.g., `Class`, `Method`, `Property`).
- `AllowMultiple`: Indicates whether the attribute can be applied multiple times.
- `Inherited`: Specifies if the attribute is inherited by derived classes.

```c#
[AttributeUsage(AttributeTargets.Method, AllowMultiple = false, Inherited = true)]
public class ExampleAttribute : Attribute
{
    public string Info { get; }

    public ExampleAttribute(string info)
    {
        Info = info;
    }
}

```
### Built-In Attributes in C-Sharp

Some commonly used attributes in C# include:

- `[Serializable]`: Marks a class as serialisable.
- `[Obsolete]`: Marks code as outdated, providing a warning or error.
- `[DllImport]`: Imports a DLL function (used in interop).
- `[HttpGet]`, `[HttpPost]`: Used in ASP.NET for defining HTTP routes.

### Summary

1. Attributes in C# are metadata applied using square brackets.
2. They are implemented as classes derived from `System.Attribute`.
3. Custom attributes are defined using constructors and optional properties.
4. Attributes can modify compile-time behaviour or runtime functionality, often read using reflection.
5. They provide extensibility for frameworks and tools, enabling more declarative and clean code.

## What is a virtual method?

The `virtual` keyword declares that the method can be overrided using the `override` keyword in child classes.

This is the main way to introduce default methods to your parent class.

```c#
// In parent class
public virtual void DoSomething
{
...
}

// In child class
public override void DoSomething
{
...
}
```

### Can default methods be introduced in other ways?

**You can achieve similar behaviour:**
- **Declare a method with the same signature as the parent's method in the child class** - this will hide the base class method when called on an instance of the derived class (you can still access in using the `base` keyword)
- **Use interfaces** - both the base class and derived classes can implement this interface.