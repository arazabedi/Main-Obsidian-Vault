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
	- Parallelinq (`ParallelQuery<T>`, `ParallelEnumerable`)
	- Plenty of other LINQ providers (many technologies - especially Microsoft ones such as Sharepoint have a LINQ provider allowing you to use LINQ queries on them)
- LinkPad is awesome (but not on Mac)
# 2. LINQ Operators in Detail

## Overview

**LINQ provides a large set of operators:**
- Projection
- Filtering
- Sorting
- Set Ops
- Quantifier Ops
- Grouping
- Element Ops
- Converting Types
- Concatenation
- Aggregation

>[!note]
>- Some Operators are *immediate*
>	- They iterate the entire collection as soon as they are invoked
>	- Examples: `Count()`, `ToArray()`
>	- Iteration a collection more than once is a bad idea (Rider will warn against this)
>- Some operators are *deferred*
>	- Calculations will only happen when you iterate the collection
>	- E.g. `Enumerable.Range(1,10).Select(rand.Next(10)` will yield a *completely different* sequence of random numbers each time it is computed.
>	- The above code creates a *generator* , not the random sequence itself, hence every time `foreach` is called, the generator will generate a new sequence for the `foreach` to loop through.
>	- Deferred operations can be:
>		- *Streaming* - do not have to read all data before elements are yielded
>		- *Non-streaming* - must read all source data before they yield an element
>- Consult MSDN documentation on how an operator behaves (deferred or immediate etc.)

## Conversion Operators

### Casting old data structures so that they can work with LINQ

ArrayList is an old data structure with doesn't even implement `IEnumerable<T>`:

```c#
var list = new ArrayList();
list.Add(1);
list.Add(2);
list.Add(3);
// this won't work
Console.WriteLine(list.Select(i => (int)i).Sum());
```

But amazingly the `cast` operator does work:

```c#
Console.WriteLine(list.Cast<int>().Sum());
```

`.Cast()` will return a compiler-generated internal iterator (`<CastIterator>`)  which can be summed using the `.Sum()` method. In fact, you can use any link method on it as the iterator method implements the `IEnumerable<T>` interface.

You can also loop through an iterator.

```c#
var thing = list.Cast<int>();

foreach (var it in thing)
{
    Console.WriteLine(it);
}
```

>[!warning]
>`.Cast<int>()` is not the same thing as `(int)`. You cannot simply take an ArrayList of integers and use `.Cast<double>`. This would be an `InvalidCastException`. The `Cast()` LINQ method is used to cast weakly-typed collections, not for casting data types from one to another.

### Materialising Data Structures

In order to get a particular data structure out of an iterator (such as the one created by the `Enumerable.Range()` method), you can use:

- `ToString()`
- `ToDictionary()`
- `ToList()`
- `ToArray()`
- `ToHashSet()`
- `ToSortedSet()`
- `Aggregate()` - see below:

```c#
var range = Enumerable.Range(1, 5);
var sum = range.Aggregate((x, y) => x + y); // Sums the numbers
```

```C#
var numbers = Enumerable.Range(1,10);
var arr = numbers.ToArray(); // ToList()
var dict = numbers.ToDictionary(i => (double)i/10, i => i%2 == 0);
Console.WriteLine(arr);
Console.WriteLine(dict);
```

### Casting to different interfaces

This is not that useful but hey-ho.

```c#
var arr2 = new[]{1,2,3};
var arre = arr2.AsEnumerable(); // simply casts to IEnumerable<int>
ParallelQuery<int> pq = arr2.AsParallel();
// AsQueryable() only really useful for mock objects in testing
```

## Projection Operators (most used)

### `.Select()` - apply functions to sequences (map)

Transformation (or JavaScript-like `map`) - apply a function to each element:

```c#
var numbers = Enumerable.Range(1, 4);
var squares = numbers.Select(x => x*X);

// 1, 4, 9, 16
```

```c#
string sentence = "This is a sentence";
var wordArray = sentence.Split()
// [This, is, a, sentence]
var wordLengths = wordArray.Select(w => w.Length);
// 4, 2, 1, 8
```

Using anonymous types:

```c#
var wordsWithLength = sentence.Split()
						.Select(w => new { w, w.Length});
// [[this, 4], [is, 2], [a, 1], [nice, 4], [sentence 8]]
```

Select can do anything it wants. It doesn't have to use values given by the iterator:

```c#
var randomNumbers = Enumerable.Range(1, 10).Select(_ => rand.Next(10));
```
### `.SelectMany()` - map + flatten/combine sequences

Flatten a data structure containing arrays:

```c#
var sequences = new [] {"red, green, blue", "orange", "white, pink"};
var allWords = sequences.SelectMany()
// red, green, blue, orange, white, pink
// remember the above is an IEnumerable<String>
```

Generate combinations of collections:

```c#
string [] objects = {"house", "car", "bicycle"};
string [] colors = {"red", "green", "gray"};
var pairs = colors.SelectMany(_ => objects, (c, o) => $"{c} {o}")
//or
var pairs = colors.SelectMany(_ => objects, (c, o) => new { Color = c, Obj = o});
```

Explanation of the combination generation:
- `SelectMany` projects each element of the collection into another sequence
- `_ => objects` is a lambda function that specifies that for each element in the `colors` combination, the `objects` collection will be used to generate combinations.
- THE `_` is a placeholder because the first parameter (each element of `colors`) is not used here.
- `(c, o) => $"{c} {o}"` specifies how the elements of the two collections should be combined.
- The result of `SelectMany` is flattened at the end.

>[!Further Explanation]
>`SelectMany` has a number of overloads.
>
>1. `source` - the original collection being operated on (`colors`).
>2. `collectionSelector` - a function that takes each element of the `source` collection and returns a new collection (`objects`).
>	- Tells `SelectMany` how to "expand" each element of the outer sequence into an inner sequence
>3. `resultSelector` - a function that takes
>	1. An element from the original collection (`c` from `colors`)
>	2. An element from the new collection (`o` from `objects`)
>	3. And returns the desired projection/result (e.g. `Red Ball`)
>
>```csharp
>var pairs = colors.SelectMany(_ => objects, (c, o) => $"{c} {o}");
>```
>
>The two lambdas correspond to the `collectionSelector` and `resultSelector` parameters:
>1. `_ => objects` (**Collection Selector**):
>	- For each element of `colors`, this function returns the entire `objects` collection.
>	- The `_` placeholder implies that we don't care about the current element of `colors` in this function; it always returns `objects`
>2. **`(c, o) => $"{c} {o}"` (Result Selector)**:
>	- Combines the elements of the outer sequence (`c` from `colors`) and inner sequence (`o` from `objects`) into a single result.
>
>Thus, these are two distinct lambda functions, not a single lambda function.

Basically, the second parameter gives you control over the final result of SelectMany. In this case we've used it to output the combinations.

## Filtering Operations

### Where - filter using lambda function

```c#
var numbers = Enumerable.Range(1, 10);
var evenNumbers = numbers.Where(n => n % 2 = 0);
// 1, 2, 3, 4, 5, 6, 7, 8, 9, 10
```
### Combining Where and Select

```c#
var oddSquares = numbers.Select(x => x*x).Where(y => y %2  == 1)
// 1, 9, 25, 49, 81
```

### OfType - filter for type

```c#
object [] values = { 1, 2.5, 3, 4.56};
var integerValues = values.OfType<int>();
// 1, 3
```

## Sorting Operators

## OrderBy

```c#
var rand = new Random();
var randomValues = Enumerable.Range(1, 10).Select(_ => rand.Next(10) - 5);

var csvString = new Func<IEnumerable<int>, string>(values =>
{
    return string.Join(",", values.Select(v => v.ToString()).ToArray());
});

Console.WriteLine(csvString(randomValues.OrderBy(x => x)));
```

## OrderByDescending

```c#
Console.WriteLine(csvString(randomValues.OrderByDescending(x => x)));
```

### Ordering Classes

```c#
var people = new List<Person>  
{  
    new Person(Name = "Adam", Age = 36),  
    new Person(Name = "Gary", Age = 28),  
    new Person(Name = "Kyle", Age = 42)  
};

OrderBy(p => p.Name)
OrderBy(p => p.Age)
// result is of type <IOrderedEnumerable>
```

### Chain Ordering

```c#
OrderBy(p => p.Age).OrderByDescending(p => p.Name)
// You can chain ordering
```

### Reverse (Useful for strings)

```c#
string s = "this is a test";
var reversedString = s.Reverse().ToArray();
// tset a si sihT
```

## Grouping Operators

Split a dataset into particular groups by key.

```c#
var people = new List<Person>  
{  
    new Person(Name = "Adam", Age = 36),  
    new Person(Name = "Gary", Age = 28),  
    new Person(Name = "Adam", Age = 42)  
};

var byName = people.GroupBy(p => p.Name)

// or 

IEnumerable<IGrouping<string, Person>> byName = people.GroupBy(p => p.Name)

var byNameArray = byName.ToArray();  
  
foreach (var group in byNameArray)  
{  
    Console.WriteLine($"Group: {group.Key}");  
    foreach (var person in group)  
    {        Console.WriteLine($"  {person.Name} ({person.Age})");  
    }}

/* Group: Alice
  Alice (25)
Group: Bob
  Bob (30)
  Bob (19)
  Bob (31)
Group: Charlie
  Charlie (35)
Group: Kyle
  Kyle (42)
*/
```

You could also flatten the grouping using `SelectMany`:

```c#
var groupedContents = byName
            .SelectMany(group => group, (group, person) => new { GroupKey = group.Key, Person = person })
            .ToArray();

foreach (var item in groupedContents)  
{  
    Console.WriteLine($"Group: {item.GroupKey}, Name: {item.Person.Name}, Age: {item.Person.Age}");  
}
```

>[!note]
>`IGrouping` is a specialised LINQ interface similar to a dictionary.
### Group by age:

```c#
GroupBy(p => p.Age < 30);
```
### Group by a particular key but only keep the value data

```c#
GroupBy(p => p.Age, p => P.Name)
// Second parameter decides what information to keep
```

### `IGrouping` properties (how to get its key and values)

```c#
foreach (var grouping in enumerable) 
{
	Console.WriteLine(grouping.Key + ":");
	foreach (var item in grouping) 
	{
		Console.WriteLine(item);
	}
}
```

## Set Operators

### Set Operators Summary

```c#
string word1 = "helloo";
string word2 = "help";

word1.Distinct()
// h, e, l, o

word1.Intersect(word2)
// h e l

word1.Union(word2)
// h e l o p

word1.Except(word2)
// o
```

String implements `IEnumerable<char>` therefore it can be looped over:

```c#
foreach (char c in myString)
{
    Console.WriteLine(c);
}
```
### Distinct - get every unique letter

Related to Set Theory in Mathematics

```c#
string word1 = "helloo";

word1.Distinct()
// h, e, l, o
```
### Intersection - get common letters

```c#
string word1 = "helloo";
string word2 = "help";

word1.Intersect(word2)
// h e l
```
### Union - get all letters in two words

```c#
string word1 = "helloo";
string word2 = "help";

word1.Union(word2)
// h e l o p
```

### Except - only letters in the first word (not in the other)

```c#
string word1 = "helloo";
string word2 = "help";

word1.Except(word2)
// o
```

## Quantifier Operators

Let you determine whether 1 or more elements in a collection satisfies a particular predicate.

### `All` - Check if all elements satisfy a condition

```c#
int [] numbers = { 1, 2, 3, 4, 5}
var areTheyAllGreaterThanZero = numbers.All(x => x > 0) // takes a predicate and checks whether ALL the elements satisfy the predicate, returning a boolean
// True
var areTheyAllOdd = numbers.All(x => x % 2 == 1)
// False
```

### Any - Check if any of the elements satisfies  a condition

```c#
int [] numbers = { 1, 2, 3, 4, 5}
var anyNumbersLessThanTwo = numbers.Any(x => x < 2);
// True
```

### Any without arguments - Check if the collection has any elements (is not empty)

```c#
int [] numbers = { 1, 2, 3, 4, 5}
var isEmpty = numbers.Any();
// False
```

### Contains - check whether an IEnumerable contains a particular value

```c#
int [] numbers = { 1, 2, 3, 4, 5}
numbers.Contains(5);
// True
```

## Partitioning Data

Given a stream of data, you might want to ignore certain items.

### Skip - skip the first given number of elements

```c#
var numbers = new[] { 3,3,2,2,1,1,2,2,3,3 };

Console.WriteLine(numbers.Skip(2));
// 2 2 1 1 2 2 3 3 basically it skips 3 3 
```

### Take - Put a limit on the number of items you take from a collection

```c#
var numbers = new[] { 3,3,2,2,1,1,2,2,3,3 };

Console.WriteLine(numbers.Take(2));
// 33
```

### SkipWhile - skip IF condition holds at the start of the collection

```c#
var numbers = new[] { 3,3,2,2,1,1,2,2,3,3 };

Console.WriteLine(numbers.SkipWhile(i => i ==3 ));
// 2 2 1 1 2 2 3 3

Console.WriteLine(numbers.SkipWhile(i => i == 2 ));
// 3 3 2 2 1 1 2 2 3 3

Console.WriteLine(numbers.SkipWhile(i => i >= 2 ));
// 1 1 2 2 3 3 

```

### TakeWhile - take UNTIL the condition fails

```c#
var numbers = new[] { 3,3,2,2,1,1,2,2,3,3 };

Console.WriteLine(numbers.TakeWhile(i => i == 3 ));
// 3 3
```

## Join Operators - collect data from multiple separate sources

### Join - Match object values

```c#
using Playground;
class Person
{
    public string Name { get; set; }
    public string Email { get; set; }

    public Person(string name, string email)
    {
        Name = name;
        Email = email;
    }
}

class Record
{
    public string Mail, SkypeId;
    
    public Record(string mail, string skypeId)
    {
        Mail = mail;
        SkypeId = skypeId;
    }
}

class Program
{
    static void Main()
    {
        var people = new Person[]
        {
            new Person("Jane", "jane@foo.com"),
            new Person("John", "john@foo.com"),
            new Person("Chris", string.Empty),
        };

        var records = new Record[]
        {
            new Record("jane@foo.com", "janeAtFoo"),
            new Record("jane@foo.com", "janeAtHome"),
            new Record("john@foo.com", "john1980"),
        };

        var query = people.Join(records,
            person => person.Email, // person could be anything (e.g. x)
            record => redcord.Mail, // record could be anything (e.g. x)
            ((person, record) => new {Name = person.Name, SkypeId = record.SkypeId })
        );

        Console.WriteLine(query.JoinToString());

		foreach (var item in query)
        {
            Console.WriteLine($"Name: {item.Name}, SkypeId: {item.SkypeId}");
        }
    }
}
```

`Join` can take four parameters, and acts one one collection.

```c#
outerCollection.Join(
    innerCollection, // The collection to join with
    outerKeySelector, // Key selector for the outer collection
    innerKeySelector, // Key selector for the inner collection
    resultSelector  // Function to project the result
);
```

**Concrete example of `Join`:**

```c#
var query = people.Join(
    records,                           // The inner collection to join with
    person => person.Email,            // Outer key selector
    record => record.Mail,             // Inner key selector
    (person, record) => new {          // Result selector
        Name = person.Name,
        SkypeId = record.SkypeId
    }
);
```

A _join_ of two data sources is the association of objects in one data source with objects that share a common attribute in another data source.

For example, if two classes both have a property of `Name`, you can find all the pairs of objects with matching `Name` values.

### GroupJoin - Join that groups matches in arrays

```c#
var grouped = people.GroupJoin(records, x => x.Email, y => y.Mail, (person, record) => new {  
    Name = person.Name,  
    SkypeIds = record.Select(x => x.SkypeId).JoinToString()  
});
```

>[!Explanation]
>In the above code, the `GroupJoin` operation associates each person with a *collection of matching records* (not a single record). Therefore `record` in the lambda function represents an enumerable of `Record` objects, not a single record. 
>
>Trying to access record.SkypeId would result in a compiler error. 
>
>In order to extract `SkypeId` values, we need to use a `.Select(x => x.SkypeId)` to project the collection of Record objects into their `SkypeId` values, which are then concatenated with `.JoinToString()`.

**`GroupJoin` signature:**

```c#
IEnumerable<TResult> GroupJoin<TOuter, TInner, TKey, TResult>(
    IEnumerable<TOuter> outer,
    IEnumerable<TInner> inner,
    Func<TOuter, TKey> outerKeySelector,
    Func<TInner, TKey> innerKeySelector,
    Func<TOuter, IEnumerable<TInner>, TResult> resultSelector
)
```

## Equality Operation - how to compare collections for their content

### SequenceEqual - check that two sequences are the same

```c#
var arr1 = new[]{1, 2, 3};
var arr2 = new[]{1, 2, 3};

Console.WriteLine(arr1.SequenceEqual(arr2));
// true
```

>[!tip]
>This works regardless of the underlying collection.

### Unit-Testing LINQ Methods

#### EqualTo 

```c#
Assert.That(a, Is.EqualTo(c));
```
#### EquivalentTo

```c#
Assert.That(a, Is.EquivalentTo(c));
```

## Element Operators

Element operations return a single value.

### First - get the first element that satisfies the condition

If there is no condition then this method just returns the first element in the collection.

```c#
var numbers = new List<int>{1,2,3};
Console.WriteLine(numbers.First()); // 1
Console.WriteLine(numbers.First(x => x > 2)); // 3
```

>[!warning]
>You will get an `InvalidOperationException` if your predicate has no matches (i.e. if no elements satisfy the predicate). Solve this using the `FirstOrDefault` method.

### FirstOrDefault - returns the default value of the type if no value in the collection satisfies the predicate

```c#
var numbers = new List<int>{1,2,3};
Console.WriteLine(numbers.First(x => x > 10)); // 0
```

### Last 

```c#
var numbers = new List<int>{1,2,3};
Console.WriteLine(numbers.Last()); // 3
Console.WriteLine(numbers.Last(x => x < 3)); // 2
```

### Single - expects one element in the collection

```c#
var numbers = new List<int>{1,2,3};
Console.WriteLine(numbers.Single()); // InvalidOperationException

var singleNumber = new List<int>{1};
Console.WriteLine(numbers.Single()); // 1
```

### SingleOrDefault - Find out whether there is one item in the collection OR if its empty

```c#
var numbers = new List<int>{1,2,3};
Console.WriteLine(numbers.SingleOrDefault()); // InvalidOperationException

var emptyNumbers = new List<int>{};
Console.WriteLine(emptyNumbers.SingleOrDefault()); // 
// 0
```

>[!warning]
>`SingleOrDefault` will only give the default value for a type if the collection is empty. Otherwise it will throw an `InvalidExceptionError`.

### ElementAt - Find element at index given

```c#
var numbers = new List<int>{1,2,3};
Console.WriteLine(numbers.ElementAt(1))
// 2
```

>[!warning]
>`ElementAt` is an expensive operation as it loops through the `IEnumerable` until it reaches the appropriate index. Use the square bracket operator where possible, such as in arrays (e.g. `arr[2]`).
### ElementAtOrDefault - gives the default value for a type if no element found at given index

```C#
var numbers = new List<int>{1,2,3};
Console.WriteLine(numbers.ElementAt(4))
// 0
```

## Concatenation Operators


## Simple Operator Examples

In C#, iterator objects, such as those returned by LINQ methods (like `.Cast<T>()`), implement the `IEnumerable<T>` interface, which provides several methods you can use to work with collections or sequences of data. These iterator objects are not just for `foreach` loops; they come with a rich set of LINQ extension methods that allow you to perform various operations on them.

Here are several things you can do with an iterator object returned from LINQ:

### 1. **Aggregation and Summarization**

- **`.Sum()`**: Sums up all the elements in the collection.
    
    ```csharp
    var sum = myCollection.Sum();
    ```
    
- **`.Average()`**: Computes the average of the elements.
    
    ```csharp
    var average = myCollection.Average();
    ```
    
- **`.Min()`** / **`.Max()`**: Finds the minimum or maximum value in the collection.
    
    ```csharp
    var min = myCollection.Min();
    var max = myCollection.Max();
    ```
    

### 2. **Filtering**

- **`.Where()`**: Filters the collection based on a condition.
    
    ```csharp
    var evenNumbers = myCollection.Where(x => x % 2 == 0);
    ```
    
- **`.First()`** / **`.FirstOrDefault()`**: Returns the first element that matches a condition or the default value if none is found.
    
    ```csharp
    var firstEven = myCollection.First(x => x % 2 == 0);
    ```
    

### 3. **Projection (Transformation)**

- **`.Select()`**: Projects each element of the collection into a new form.
    
    ```csharp
    var squares = myCollection.Select(x => x * x);
    ```
    
- **`.SelectMany()`**: Flattens a collection of collections into a single collection.
    
    ```csharp
    var flattened = myCollection.SelectMany(x => x.Children);
    ```
    

### 4. **Sorting**

- **`.OrderBy()`** / **`.OrderByDescending()`**: Sorts the elements in ascending or descending order.
    
    ```csharp
    var sorted = myCollection.OrderBy(x => x);
    var sortedDescending = myCollection.OrderByDescending(x => x);
    ```
    
- **`.ThenBy()`** / **`.ThenByDescending()`**: Performs a secondary sort after the primary sort.
    
    ```csharp
    var sortedComplex = myCollection.OrderBy(x => x.Age).ThenBy(x => x.Name);
    ```
    

### 5. **Set Operations**

- **`.Distinct()`**: Removes duplicate elements from the collection.
    
    ```csharp
    var distinctItems = myCollection.Distinct();
    ```
    
- **`.Union()`**: Combines two collections, excluding duplicates.
    
    ```csharp
    var combined = collection1.Union(collection2);
    ```
    
- **`.Intersect()`**: Returns only the elements that are common to both collections.
    
    ```csharp
    var commonItems = collection1.Intersect(collection2);
    ```
    
- **`.Except()`**: Returns elements that are in the first collection but not in the second.
    
    ```csharp
    var difference = collection1.Except(collection2);
    ```
    

### 6. **Grouping**

- **`.GroupBy()`**: Groups elements of the collection by a key.
    
    ```csharp
    var grouped = myCollection.GroupBy(x => x.Category);
    ```
    

### 7. **Quantifiers**

- **`.Any()`**: Checks if any elements in the collection satisfy a condition.
    
    ```csharp
    bool hasEven = myCollection.Any(x => x % 2 == 0);
    ```
    
- **`.All()`**: Checks if all elements satisfy a condition.
    
    ```csharp
    bool allPositive = myCollection.All(x => x > 0);
    ```
    
- **`.Contains()`**: Checks if a specific element exists in the collection.
    
    ```csharp
    bool hasFive = myCollection.Contains(5);
    ```
    

### 8. **Element Retrieval**

- **`.ElementAt()`**: Gets the element at a specific index.
    
    ```csharp
    var elementAtIndex = myCollection.ElementAt(3);
    ```
    
- **`.Skip()`** / **`.Take()`**: Skips a certain number of elements or takes a specified number of elements from the collection.
    
    ```csharp
    var firstThree = myCollection.Take(3);
    var afterFirstThree = myCollection.Skip(3);
    ```
    

### 9. **Materializing the Iterator**

Sometimes, it's useful to "materialize" the iterator into a list, array, or other collection types for further manipulation.

- **`.ToList()`**: Converts the iterator into a `List<T>`.
    
    ```csharp
    var list = myCollection.ToList();
    ```
    
- **`.ToArray()`**: Converts the iterator into an array.
    
    ```csharp
    var array = myCollection.ToArray();
    ```
    

### 10. **Concatenation**

- **`.Concat()`**: Combines two collections by appending one to the other.
    
    ```csharp
    var combined = collection1.Concat(collection2);
    ```
    

### 11. **Partitioning**

- **`.TakeWhile()`**: Takes elements from the collection as long as a condition is met.
    
    ```csharp
    var takeWhileEven = myCollection.TakeWhile(x => x % 2 == 0);
    ```
    
- **`.SkipWhile()`**: Skips elements from the collection as long as a condition is met.
    
    ```csharp
    var skipWhileEven = myCollection.SkipWhile(x => x % 2 == 0);
    ```
    

### 12. **Conversion Methods**

- **`.Cast<T>()`**: Converts elements in a collection to a specified type (`T`).
    
    ```csharp
    var casted = myCollection.Cast<int>();
    ```
    
- **`.ToDictionary()`**: Converts a collection to a dictionary based on a key selector and value selector.
    
    ```csharp
    var dictionary = myCollection.ToDictionary(x => x.Key, x => x.Value);
    ```
    

---

### Deferred Execution

One key feature of LINQ iterators is **deferred execution**. The query is not executed when it is defined but when it is enumerated (e.g., in a `foreach` loop, or when methods like `.ToList()` or `.ToArray()` are called). This allows LINQ to be very efficient by not doing extra work until necessary.

### Summary

You can do a lot more than just loop over iterator objects in C# LINQ. These objects can be filtered, transformed, aggregated, sorted, grouped, and combined using the various LINQ extension methods available in `System.Linq.Enumerable`. These methods are powerful tools for manipulating and querying collections.
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

## 3. Why are LINQ queries chain-able?

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

### In Depth `yield`

The `yield` keyword in C# is what enables the **deferred execution** of elements in an enumerable sequence. If you remove the `yield` statement from your code, the method `GetEvenNumbers` will not compile. Here's why:
#### Why `yield` is necessary

1. **Deferred Execution with Iterators**: The `yield return` keyword allows a method to be an **iterator**. An iterator is a special kind of method that generates elements of a sequence one at a time, as they are requested. When `yield return i` is executed, the current value of `i` is returned to the caller, and the method's execution state is preserved (paused) until the next iteration.
    
2. **Without `yield`**: If you remove `yield return`, the method `GetEvenNumbers` has no way to "pause" or "return" multiple values over time. Instead, a method that generates a sequence of numbers would need to build and return the entire sequence explicitly (e.g., as a list or array). For example, you would need to write:
    
    ```csharp
    IEnumerable<int> GetEvenNumbers(int max)
    {
        List<int> result = new List<int>();
        for (int i = 0; i <= max; i++)
        {
            if (i % 2 == 0)
                result.Add(i);
        }
        return result;
    }
    ```
    
    This version creates the list of even numbers all at once and then returns it. Unlike the version with `yield`, this approach **does not use deferred execution**; it computes the entire list before returning.
    
#### Why doesn't it work without `yield`?

- The method signature `IEnumerable<int> GetEvenNumbers(int max)` declares that it will return an **enumerable** sequence.
- Without `yield`, there’s no way to return a sequence element-by-element.
- Simply using `return` would only allow you to return a single object (e.g., a single number or a collection), not an ongoing stream of values.

For instance, replacing `yield return i` with `return i;` would cause a compilation error, as `return` in this context cannot satisfy the expected `IEnumerable<int>` return type.
#### Summary

- **With `yield`**: The method becomes an iterator, generating values on demand one at a time.
- **Without `yield`**: The method cannot fulfill its purpose of returning an enumerable sequence without explicitly building and returning a full collection.
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

When a method with `yield` is compiled:

- The compiler generates a state machine class that implements the  `IEnumerable<T>` and `IEnumerator<T>` interfaces
- The state machine keeps track of the method's state (local variables, current position, etc.).
- When `yield return` is encountered, the current state is saved, and the state machine resumes execution the next time the enumerator is called.

## 7. What is an anonymous type?

Anonymous types provide a convenient way to encapsulate a set of read-only properties into a single object without having to explicitly define a type first. The type name is generated by the compiler and is not available at the source code level. The type of each property is inferred by the compiler.

```c#
var v = new { Amount = 108, Message = "Hello" };

Console.WriteLine(v.Amount + v.Message);
```

Anonymous types are typically used in the [`select`](https://learn.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/select-clause) clause of a query expression to return a subset of the properties from each object in the source sequence. For more information about queries, see [LINQ in C#](https://learn.microsoft.com/en-us/dotnet/csharp/linq/).

## 8. What is the => that is always passed into LINQ methods are part of an argument?

=> is shorthand for creating an anonymous expression (also known as a lambda expression).

The syntax is:

```c#
(input parameters) => expression_or_block
```

These are all the same thing:

```c#
x => x > 2
(int x) => x > 2
(int x) => { return x > 2; }
```

It's essentially a shorthand for writing:

```c#
numbers.First(delegate(int x) { return x > 2; });
```

A lambda is a function, it takes one input and returns an output.

The first x in the above example is the parameter of the function. The `x > 2` is the body of the function.

Methods like `First` (LINQ method) take functions as arguments.

This is how `First` is defined:

```c#
public static TSource First<TSource>(this IEnumerable<TSource> source, Func<TSource, bool> predicate);
```

The lambda passed to the method is known as a predicate. The method uses the predicate to determine which element to return (it should satisfy the predicate - as in the expression inside the predicate should hold true for a particular element if it is to be returned by the `First` method).
# Other Points
- `ArrayList` is a pre-.NET 2.0 data structure that doesn't have strong-typing because generics had not been implemented yet.
# Useful Snippets

## Join Collections Extension Method (for logging to the console)

```c#
public static class Extensions
{
    // Use to join convert collections to comma-separated strings - useful for logging
    public static string JoinToString<T>(this IEnumerable<T> collection)
    {
        return string.Join(", ", collection);
    }
}
```

>[!info]
>**How Extension Methods Work**
>
>Extension methods add functionality to existing types such as collections or other objects without modifying their source code or created new derived classes.
>
>In C#, extension methods must be defined in a `static` class because the class is not meant to be instantiated (no objects from that class are needed).
>
>The method itself must also be static because it is not a member of the type being extended, it's just a method that can be called on an instance of the type. Even though it is used as though it's a normal instance method, it operates as static under the hood (it's a class method at the end of the day).
>
>The crucial element is the `this` keyword, which tells the compiler that the method is an extension method that will be called on an object of the parameter type.
>
>The this keyword in front of the first parameter (`IEnumerable<T> collection`) makes the method an extension of `IEnumerable<T>`.
>
>The `this` keyword makes the `JoinToString()` method available to any object that implements `IEnumerable<T>`.
>
>Under the hood, the method is translated by the compiler to this:
>```csharp
>string result = Extensions.JoinToString(numbers);
>```
>
>But you don't need to call it like this because the `this` keyword makes the method accessible in the context of an instance.

### Key points to remember:

- **No modification of original types**: The power of extension methods is that you can add functionality to existing types (like `List<T>`, `IEnumerable<T>`, etc.) without changing their original code or structure. This is particularly useful when you want to enhance types you don’t control, like types from third-party libraries.
    
- **Intellisense support**: When you define an extension method, it will show up in the Intellisense dropdown for the type it's extending. In your case, after writing `numbers.`, the `JoinToString` method will appear as a suggestion when you have an `IEnumerable<T>`.
    
- **Non-intrusive**: Extension methods are non-intrusive. You don’t modify the class of the object you're extending, but you're still able to call the method directly on that object.
### Summary of the important parts:

- **Static method in a static class**: Extension methods need to be defined in a static class, and they must be static methods because they are not inherently part of the extended type.
- **The `this` keyword**: Marks the first parameter as the type that is being extended, making it available as an instance method on objects of that type.
- **Works on any object of the correct type**: The method is available for any object that matches the type in the `this` parameter, which is `IEnumerable<T>` in this case.
- **Syntax sugar**: Extension methods allow you to call methods on types as if they were native instance methods, even though they are statically defined elsewhere.

### Why Static?

#### The actual method call is to a class (i.e. static method)

```c#
string result = Extensions.JoinToString(numbers);
```

#### What Happens if You Try to Define Extension Methods in a Non-Static Class?

```c#
// Compiler error: Extension method must be in a static class
public class Extensions
{
    public static string JoinToString<T>(this IEnumerable<T> collection)
    {
        return string.Join(", ", collection);
    }
}
```

The compiler doesn't allow this because the extension method would just behave like an instance method. You'd have to instantiate `Extensions` to use `JoinToString` which defeats the whole point.

A method can be static without the class being static though, so why does C# insist that extension methods must be inside static classes?

Because it prevents misuse, enforces utility-only behaviour (as it's meant to be a utility class), and it helps the compiler to locate the resources it needs to resolve extension methods (as it knows that they must be in static classes).

If extension methods were allowed in non-static classes, it would muddy the line between instance methods and type extensions, potentially leading to ambiguities in method resolutions.
#### Summary: Why Must the Class Be Static?

- **Prevent instantiation:** Extension methods don’t need an instance of the containing class, so allowing instantiation would be pointless and confusing.
- **Maintain clarity:** A `static` class enforces the "utility" nature of the extension methods and avoids mixing them with non-static members.
- **Simplify compiler logic:** The compiler can reliably locate extension methods within `static` classes, avoiding potential conflicts with instance methods or state.