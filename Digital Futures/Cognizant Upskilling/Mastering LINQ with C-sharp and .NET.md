LINQ is a query syntax in C# that can be applied to in-memory data structures as well as other data sources such as databases.

We'll discuss:
- LINQ operations - operators and common scenarios
- Applied LINQ - LINQ to Entities, LINQ to XML etc.
- Parallel LINQ - Multicore computing
- Extra topics - such as tool support, MoreLINQ.

The course uses LINQPAD but it's not out on Mac yet.

Pre-requisites:
- C# knowledge (extension methods, lambdas)
- Databases knowledge
- Basic TPL understanding - see the parallel programming course from the same guy
```table-of-contents
```
# 1. Getting Started With LINQ

## Overview
- How LINQ Works
-  `IEnumerable<T>`
- LINQPAD
- Generation Operations
## How LINQ Works

- LINQ = Language Integrated Query
- A technology for searching and filtering data sets:
	- LINQ to Objects: for working with arrays, lists, etc.
	- LINQ to Entities: for databases
	- LINQ to XML
	- LINQ to JSON
	- LINQ to... (e.g. Sharepoint)
- Why use LINQ? Because for/foreach loops are very tedious.

For Loop:

```c#
var results = new List<int>()
foreach (int x in input)
{
	int y - x*x;
	if (y>10)
		results.Add(y);
}
return results
```

LINQ:

```C#
return input.Select(x => x*x)
			.Where(y => y > 10)
			.ToList();
```

**How is this magic possible?**
- LINQ is implemented in two different ways:
	- Extension methods (operators)
	- Query expression syntax - SQL-like syntax directly embedded into C# (not covered in this course)
	- LINQ operators are ***higher-order functions***:
		- Functions taking functions
		- For example, to filter out elements, a LINQ operator takes a predicate (i.e. a `Func<T, bool>)
		- LINQ operators are functions that that another function (a predicate) which itself takes an element of type T and returns a boolean
	- LINQ operators present a fluent interface - they take an `IEnumberable` and return an `IEnumerable`, which makes them chain-able.
		- `foo.Where(...).Select(...).ToArray()`

Query Expression Syntax:

```c#
var query = from person in people
            where person.Age > 18
            select person.Name;
```

Method Syntax:
```c#
var query = people.Where(person => person.Age > 18)
                  .Select(person => person.Name);
```

>[!warning]
>Some operators in the method syntax are not available in method (SQL-like) syntax. Some features are not available in the method syntax (such as the `let` keyword).

## `IEnumerable<T>`

- The central interface of LINQ operations.
	- Has a weakly-typed `IEnumerable` counterpart (we won't use this - most collections are strongly typed).
	- Certain LINQ operations can convert between strong and weak type collections. 
- Implemented by all .NET collection types (`array`, `List<T>`, etc.)
	- Is support for iteration (traversing the collection and getting one element after another).
- LINQ operators are implemented as extension methods on:
	- `IEnumerable<T>`, taking `Func<>` arguments (this is called ***LINQ to Objects*** and is used for in-memory objects).
	- `IQueryable<T>` taking `Expression<>` arguments (this is used in e.g. ***LINQ to Entities (EF)*** for databases and remote connections).

**`IEnumerable<T>` is Special**
- Used for `foreach`
- Methods that return `IEnumerable` or `IEnumerator` (its counterpart method) are considered iterator methods:
	- If you have a function returning one of these it can use:
		- `yield return x;` to return a single iterated value.
		- `yield break;` to break the operation

**What is Enumerable?**
- A concrete class - NOT an interface
- Contains:
		- Static generator methods for making sequences (e.g. `Enumerable.Range()`)
		- LINQ extension method definitions (extending `IEnumerable<T>`)
		- These extensions methods are in fact LINQ!

>[!attention]
>This part - about Enumerable being the class which contains the LINQ functions - is important!

## Implementing `IEnumerable<T>`

**How can you implement `IEnumerable<T>`?**

- Inherit from Collection - this is simple because Collection already has the required methods

```c#
class Foo : Collection<int> {}
```

- Alternative - requires some work to implement the required methods:

```c#
public class Params : IEnumerable<int>
{
	private a, b, c;

	public Params(int a, int b, int c) 
	{
		this.a = a;
		this.b = b;
		this.c = c;
	}
	
	public IEnumerator<int> GetEnumerator()
	{
		yield return a;
		yield return b;
		yield return c;
	}
	
	IEnumerator IEnumerable.GetEnumerator()
	{
		return GetEnumerator();
	}
}

class Program
{
	static void Main(string[] args)
	{
		var p = new Params(1, 2, 3);
		
		foreach (var x in p)
		{
			Console.WriteLine(x)
		}
	}
}
```

- Another alternative that exposes `IEnumerable<T>` from a field/accessor rather than a type/class:

```c#
public class Person
{
    private string firstName, middleName, lastName;


    public Person(string firstName, string middleName, string lastName)
    {
        this.firstName = firstName;
        this.middleName = middleName;
        this.lastName = lastName;
    }

    public IEnumerable<string> Names
    {
        get
        {
            yield return firstName;
            yield return middleName;
            yield return lastName;
        }
    }
}

class Program
{
    public static void Main()
    {
        Person p = new Person("John", "Quincy", "Adams");
        foreach (string name in p.Names)
        {
            Console.WriteLine(name);
        }
    }
}
```

This solution involves creating new property with a getter that uses the `yield` keyword to return all the properties that are to be looped over.

This works because the `foreach` parameter accepts an `IEnumerable<T>`  as part of the argument. When the `Names` property is accessed in a `foreach` loop, it generates an **enumerable sequence** of strings, allowing iteration over `firstName`, `middleName`, and `lastName`.

If you want:
- An object to be enumerable: make it inherit from `IEnumerable<T>`
- A property which just returns a number of values one after another, make its type `IEnumerable<T>` and use `yield return [property];` & `yield break;`
- To create a class that extends a collection - then don't worry as it will automatically be an enumerable

## LINQPAD

It's not on MacOS so I'm using NetPad instead.

LINQPad/NetPad are like IDEs except you typically only have a single query or line opened.

It's a playground for you to test out individual chunks of code.

## Generation Operations

We now now that the key interface for LINQ to Objects is `IEnumerable<T>`.

All the LINQ methods are defined in a static class called `Enumerable` (we've mentioned this before).

LINQ consists of a bunch of extension methods (you can see the `this` keyword in the source code of `Enumerable`).

**How is LINQ implemented?**
- Static class `Enumerable` 
- ...contains LINQ methods 
- ...which are extension methods on `IEnumerable<[insert type here]>`
- BUT - some methods are not extension methods (mainly for generation purposes)

**Non-extension LINQ methods**

1. `Empty` - Create an empty `IEnumerable` object of the type given in the elbow brackets

```c#
Enumerable.Empty<int>()

// empty IEnumerable<int>
```

2. `Repeat` - Creates an `IEnumerable` object with the elements equal to the 1st argument given, with a quantity of the second argument given:

```c#
Enumerable.Repeat("hello", 3)

// hello hello hello
```

>[!tip]
>The above two are a bit useless.

3. `Range` - Creates an `IEnumerable` object with elements from one number to another.

```c#
Enumerable.Range(1,10)
 
// 1 2 3 4 5 6 7 8 9 10


// Extra example:

Enumerable.Range('a', 'z' - 'a').Select(c => (char) c)

// a b c d e f g h i j k l m n o p q r s t u v w x y z
// We need to use select because it will return a sequence of integers - not characters. Select allows us to cast them back to characters.
```

## Summary

- LINQ is a set of functions/operators which operate on collections (any data which can be represented as an `IEnumerable` or similar interface)
- Has two syntax implementations
	- Extensions methods
	- Query syntax (very similar to SQL but not covered in this course)
- Enumerable static class
	- Implementation of LINQ extension methods on `IEnumerable<T>`
	- Some useful generators (most importantly, `Enumerable.Range`)
- Different varieties of LINQ
	- LINQ to Objects (`IEnumerable<T>`, `Enumerable`)
	- LINQ to Entities (`IQueryable<T>)` - using the Entity Framework (an ORM)
	- LINQ to XML (`System.Xml.Linq`) - different namespace
	- Parallellinq (`ParallelQuery<T>`, `ParallelEnumerable`)
	- Plenty of other LINQ providers (many technologies - especially Microsoft ones such as Sharepoint have a LINQ provider allowing you to use LINQ queries on them)
- LinkPad is awesome (but not on Mac)
# 2. LINQ Operators in Detail

# Q & A

## 1. How is LINQ implemented?

LINQ consists of extension methods defined in the Enumerable and Queryable classes (`System.Linq.Enumerable` & `System.Linq.Queryable`).

These methods add querying capability to types that implement `IEnumerable<T>` and `IQueryable<T>`.

## 2. What is an extension method?

A static method in a static class - but called as if it were an instance method of another type (which the static class in question extends from)

See an example of an extension method below. A static class is used, within which is a static method that takes a parameter preceded by the `this`:

```c#
public static class StringExtensions
{
    public static int WordCount(this string str)
    {
        return str.Split(new[] { ' ', '\t', '\n' }, StringSplitOptions.RemoveEmptyEntries).Length;
    }
}

// Usage:
string text = "Hello World!";
int count = text.WordCount(); // Output: 2
```

## 3. Why are Linq queries chain-able?

Because the implementation of the Linq methods takes an `IEnumerable<T>` and then returns an `IEnumerable<T>`.

## 4. What is the `yield` keyword in C#?

It allows a method to return elements of a sequence one by one instead of returning all the elements in a collection.

When a method execution hits `yield return` , the method's execution is suspended and the value after the keywords is passed to the caller.

```c#
public static IEnumerable<int> GetEvenNumbers(int max)
{
    for (int i = 0; i <= max; i++)
    {
        if (i % 2 == 0)
            yield return i; // Return each even number one at a time.
    }
}

// Usage:
foreach (int number in GetEvenNumbers(10))
{
    Console.WriteLine(number); // Outputs: 0, 2, 4, 6, 8, 10
}

```

In the above code, the caller is the `foreach` loop. Therefore when the loop calls the `GetEvenNumbers` method, numbers are returned to the loop one at a time, allowing the loop to execute the Console.WriteLine on each number in the sequence despite there not being a collection to loop through.

`yield break` is used to end the process.

## 5. What is the point of `yield`?

- **Memory Efficiency** - no collection is held in memory (making this useful for large datasets or infinite sequences)
- **Lazy Evaluation** - since each element is only produced at the request of the caller method, computational resources are not used for unused elements.
- **Simplifies Iterators** - you don't need to manually maintain the state (e.g. via `IEnumerator`). Implementing an iterator requires:
		1. Keeping track of the current position in the collection (an index or pointer)
		2. Managing the current element to return
		3. Handling the logic for when to stop iterating
		4. Storing and updating the iterator's state across successive calls to MoveNext()

>[!warning]
> **Limitations of `yield`**
>1. Cannot be used in:
>	- Methods with `ref` or `out` parameters
>	- Methods in anonymous or asynchronous lambdas
>2. Debugging can be Complex:
>	-Because of the state machine abstraction, debugging the flow of `yield`-based methods might be less intuitive.

### Manual Implementation of an iterator

Here's a manual implementation of an iterator using the `IEnumerator` interface:

```c# 
public class EvenNumbers : IEnumerable<int>, IEnumerator<int>
{
    private int _current;
    private int _max;
    private bool _started;

    public EvenNumbers(int max)
    {
        _max = max;
        _started = false;
        _current = 0;
    }

    public int Current => _current; // Return the current value.

    object IEnumerator.Current => Current;

    public bool MoveNext() // Advance to the next element.
    {
        if (!_started)
        {
            _started = true;
            _current = 0;
            return true;
        }

        _current += 2;
        return _current <= _max;
    }

    public void Reset() // Reset to initial state.
    {
        _started = false;
        _current = 0;
    }

    public void Dispose() { }

    public IEnumerator<int> GetEnumerator() => this;

    IEnumerator IEnumerable.GetEnumerator() => this;
}

// Usage:
foreach (int num in new EvenNumbers(10))
{
    Console.WriteLine(num); // Outputs: 0, 2, 4, 6, 8, 10
}

```

####  What’s Happening Here?

- You have to implement both `IEnumerable<int>` and `IEnumerator<int>`.
- You need fields to **track the state**: `_current`, `_max`, `_started`.
- You must write all the logic for:
    - Starting iteration.
    - Incrementing `_current`.
    - Checking if `_current` is within bounds.
    - Resetting the iterator if needed.

This approach works, but it’s verbose and requires careful attention to details.

#### The simpler way using `yield`

With `yield`, state management is handled by the compiler, so you can write much less code:

```c#
public static IEnumerable<int> GetEvenNumbers(int max)
{
    for (int i = 0; i <= max; i += 2)
    {
        yield return i; // Automatically saves the state.
    }
}

// Usage:
foreach (int num in GetEvenNumbers(10))
{
    Console.WriteLine(num); // Outputs: 0, 2, 4, 6, 8, 10
}

```

### How `yield` simplifies iterators

1. **No State Management**:
    - The compiler automatically generates the necessary state machine to track variables (`i` in this case) and the current position.
2. **Less Boilerplate**:
    - No need to implement `IEnumerable` and `IEnumerator` interfaces manually.
    - No `MoveNext`, `Current`, or `Reset` logic to write.
3. **Focus on Logic**:
    - You only need to focus on the logic for generating the sequence (e.g., iterating from `0` to `max` in steps of `2`).
    - You don't worry about tracking where the iteration left off.
4. **Deferred Execution**:
    - Just like with the manual iterator, the sequence is computed lazily (elements are only produced as needed).

### `yield` should be used for implementing custom iterators or sequences

`yield` is most appropriate when you want to produce elements on demand (deferred execution) or when working with large/infinite datasets/sequences or complex iteration logic.

**Summary:**
1. Big data
2. Complex sequence logic
3. Demand-based execution to save computation
4. No temporary collection needed (saves memory)
5. Works with async

```c#
public async IAsyncEnumerable<int> GetNumbersAsync()
{
    for (int i = 0; i < 5; i++)
    {
        await Task.Delay(100); // Simulate async work.
        yield return i;
    }
}

// Usage:
await foreach (int num in GetNumbersAsync())
{
    Console.WriteLine(num); // Outputs: 0, 1, 2, 3, 4
}
```

### When not to use `yield`?

1. Immediate results needed
2. Performance-sensitive scenarios - `yield` might be worse that manual state management in tight loops (lots of iterations in a short time)
3. In methods with `ref` or `out` parameters
4. For single-use collections (`List<T>` is easier)

#### Why does `yield` matter in tight loops?

1. State machine overhead - this manages the variables for the loop's state and managing transitions. Can be slower that a manual state management.
2. Enumerator calls - Each `yield return` requires interaction with the generated enumerator (e.g. calling `MoveNext()` and accessing the `Current` property).
3. Deferred Execution - the caller has to request each item, adding overhead.

For these reasons it may be worthwhile to precompute results and store them in a collection like `List<T>` for efficiency's sake.

Using `yield`:

```c#
public static IEnumerable<int> GenerateSquares(int count)
{
    for (int i = 0; i < count; i++)
    {
        yield return i * i; // Adds overhead for state management.
    }
}

// Usage:
foreach (int square in GenerateSquares(1_000_000))
{
    // Process squares
}

```

Alternative: Precomputing Results (for performance at the expense of memory):

```c#
public static List<int> GenerateSquares(int count)
{
    var squares = new List<int>(count);
    for (int i = 0; i < count; i++)
    {
        squares.Add(i * i); // Precompute all values.
    }
    return squares; // Return all at once.
}

// Usage:
foreach (int square in GenerateSquares(1_000_000))
{
    // Process squares
}

```

**So in summary, avoid `yield` in tight loops:**
1. When performance is critical
2. When memory is not an issue
3. When immediate results are necessary (e.g. for sorting or aggregation)
## 6. What is the state machine abstraction?

>[!warning]
>I don't get this if I'm honest

When a method with `yield` is compiled:

- The compiler generates a state machine class that implements the  `IEnumerable<T>` and `IEnumerator<T>` interfaces
- The state machine keeps track of the method's state (local variables, current position, etc.).
- When `yield return` is encountered, the current state is saved, and the state machine resumes execution the next time the enumerator is called.