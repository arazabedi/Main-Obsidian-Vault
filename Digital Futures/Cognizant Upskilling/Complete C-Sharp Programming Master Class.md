# Basics

## Compilation

Compile .cs file to exe:
```bash
csc [filename].cs
```

Compile .cs to cross-platform and run:

```bash
dotnet run --source [filename].cs
```

Compile .cs to cross-platform executable (dll):

```bash
dotnet build
```

(Not sure but tihnk you need to create a project first)...

Create project:

```bash
dotnet new console -o MyApp
cd MyApp
```

macOS executable:

```bash
dotnet publish -r osx-x64 --self-contained true -p:PublishSingleFile=true
```

**Run the Executable**: The executable will be in the `bin/Debug/net<version>/osx-x64/publish` directory. Run it like any macOS binary:

```bash
./MyApp
```


## Debugging

- Debugger (specific tool)
- Stepping
- Inspect variables
- Memory inspection
- Call stack inspection
- Watch expressions

Types of Mistakes:
- Syntax Error
- Logical Error
- Runtime Error
- Memory Leak (memory not released after use)

## String Manipulation

- Standard Concatenation
- Composite:
```cs
Console.WriteLine("hello, my name is {0}. Nice to meet you {1}", name, other name)
```
- Interpolations:
```cs
Console.WriteLine($"hello my id is {id}")
```
- Verbatim:
```cs
Console.WriteLine(@"The sky was a deep shade of blue,
dotted with fluffy, white clouds...)
```
Ignores escape characters, also uses exact spacing.

Methods:

- Substring:
```cs
string word = crave
word.substring(1,2) // ra
word.substring(1) // rave
```

- Lower - all lowercase
- Upper - all uppercase
- Trim - remove all trailing and leading whitespaces
- IndexOf - find the starting index of a particular substring of a string
- IsNull & WhiteSpace - returns true when it's null or there's a whitespace

## OOP

Yes, C# classes are very similar to Java classes in terms of syntax and structure because both are object-oriented programming (OOP) languages derived from C-style syntax. However, there are some notable differences and features unique to C# that you should be aware of if you're transitioning from Java. Here's a quick rundown:

---

#### **Key Similarities**
1. **Class Declaration**:
   ```csharp
   public class MyClass {
       private int myField;

       public int MyProperty { get; set; }

       public void MyMethod() {
           // Method logic
       }
   }
   ```

2. **Constructors**:
   ```csharp
   public MyClass() {
       // Default constructor
   }
   ```

3. **Inheritance and Interfaces**:
   ```csharp
   public class ChildClass : ParentClass, IMyInterface {
       // Child class implementation
   }
   ```

4. **Static Members**:
   ```csharp
   public static void StaticMethod() {
       // Static logic
   }
   ```

---

### **Differences and C#-Specific Features**

1. **Properties (Getter/Setters)**:
   - C# has a built-in syntax for properties, eliminating the need for explicit getter and setter methods.
   ```csharp
   public int MyProperty { get; set; } // Auto-implemented property
   public int AnotherProperty { get; private set; } // Read-only property
   ```

2. **`var` Keyword**:
   - C# allows type inference with `var` (but it's statically typed at compile time).
   ```csharp
   var myVariable = 42; // Compiler infers type as int
   ```

3. **Static Classes**:
   - C# allows the definition of **static classes**, which can only contain static members.
   ```csharp
   public static class Utility {
       public static void DoSomething() {
           // Static logic
       }
   }
   ```

4. **Default Implementations in Interfaces**:
   - C# interfaces can provide default method implementations.
   ```csharp
   public interface IMyInterface {
       void DoWork();
       void DefaultMethod() => Console.WriteLine("Default implementation");
   }
   ```

5. **Delegates and Events**:
   - C# uses **delegates** (function pointers) and **events** for handling callbacks and event-driven programming.
   ```csharp
   public delegate void MyDelegate(string message);
   public event MyDelegate OnMessage;
   ```

6. **LINQ (Language-Integrated Query)**:
   - C# includes LINQ for querying collections in a SQL-like syntax.
   ```csharp
   var filtered = myList.Where(x => x > 10).ToList();
   ```

7. **Extension Methods**:
   - You can add new methods to existing types without modifying them using extension methods.
   ```csharp
   public static class Extensions {
       public static int Square(this int number) {
           return number * number;
       }
   }
   ```

8. **`using` Statements**:
   - Beyond imports, `using` is also used for **resource management** to ensure deterministic disposal.
   ```csharp
   using (var reader = new StreamReader("file.txt")) {
       string line = reader.ReadLine();
   }
   ```

9. **Nullable Types**:
   - C# allows nullable value types.
   ```csharp
   int? nullableInt = null; // Nullable int
   ```

10. **Async/Await**:
    - C# has built-in support for asynchronous programming with `async` and `await`.
    ```csharp
    public async Task<int> GetDataAsync() {
        await Task.Delay(1000);
        return 42;
    }
    ```

11. **Indexers**:
    - C# classes can define indexers to access data like an array.
    ```csharp
    public string this[int index] {
        get { return data[index]; }
        set { data[index] = value; }
    }
    ```

12. **Tuples**:
    - C# provides syntax for working with tuples natively.
    ```csharp
    (int x, int y) point = (1, 2);
    ```

13. **No Checked Exceptions**:
    - Unlike Java, C# doesn't have checked exceptions, so you aren't forced to catch or declare them.

14. **Keywords**:
    - Some keywords differ:
      - `extends` (Java) → `:` (C#)
      - `implements` (Java) → `:` (C# for interfaces)
      - `final` (Java) → `sealed` (C#)

15. **Namespaces**:
    - C# uses `namespace` instead of Java's `package`.
    ```csharp
    namespace MyNamespace {
        public class MyClass { }
    }
    ```

16. **Partial Classes**:
    - C# allows splitting a single class across multiple files.
    ```csharp
    partial class MyClass {
        public void Part1() { }
    }

    partial class MyClass {
        public void Part2() { }
    }
    ```

---

### **How to Transition**
1. **Learn C#-specific features** like LINQ, extension methods, async/await, and delegates.
2. **Familiarize yourself with .NET libraries** (similar to Java's standard library).
3. Experiment with properties and resource management (`using`).
4. Explore tools like Visual Studio and .NET Core.

By leveraging your Java knowledge, you'll find C# relatively easy to pick up, with these additional features enriching your coding capabilities.

Default + auto get/set:

```csharp
public string Name { get; set; } = "Default Name";
```

Custom logic in get and set:

```cs
public class Rectangle
{
    private double width;
    private double height;

    public double Area
    {
        get { return width * height; } // Computed property
    }
}
```

Set-only or Get-only:
```cs
public class Person
{
    public string Name { get; } = "John"; // Read-only
}
```

Access modifiers on get/set:

```cs
public class Person
{
    public string Name { get; private set; }
}
```

In Java Getters and Setters are a convention used by JavaBeans and Hibernate to treat methods as properites.

In C# properties are a language feature so tools like reflection acess them as such.

| Feature                     | C#                                  | Java                               |
| --------------------------- | ----------------------------------- | ---------------------------------- |
| **Basic Syntax**            | `public string Name { get; set; }`  | `getName()` / `setName()`          |
| **Default Values**          | Supported (`= value`)               | Use field initialization           |
| **Computed Properties**     | Supported (logic in `get` or `set`) | Use methods for logic              |
| **Read-Only / Write-Only**  | Omit `set` or `get`                 | Omit setter or getter methods      |
| **Access Modifiers**        | Individual for `get` and `set`      | Modifier applies to whole method   |
| **Reflection / Frameworks** | Properties are language features    | Rely on conventions like JavaBeans |
Expression-bodied members:

Concise syntax for defining certain types of class or struct members using lambda-like syntax (=>).

Expression-Bodied Methods:

```cs
public int Square(int x)
{
    return x * x;
}

// same as

public int Square(int x) => x * x;
```

Expression-Bodied Propertied:

```cs
public int DoubleValue
{
    get { return _value * 2; }
}

// compute in an expression-bodied manner

private int _value = 10;
public int DoubleValue => _value * 2;
```

Expression-Bodied Constructors:

```cs
public Example(int id)
{
    _id = id;
}

// becomes
private int _id;
public Example(int id) => _id = id;
```

Expression-Bodied Finalizers:

```cs
class Example
{
    ~Example() => Console.WriteLine("Finalizer called");
}
```


Expression-Bodied Indexers:
```cs
class Example
{
    private int[] _values = { 1, 2, 3 };
    public int this[int i] => _values[i];
}
```

#### **When to Avoid Expression-Bodied Members**

1. **Complex Logic**: If the logic becomes too complicated, it can harm readability.
2. **Multiple Statements**: You cannot use expression-bodied syntax for more than one expression. In such cases, use a full method or property body.

![[Screenshot 2024-11-21 at 15.08.34.png]]

##### Constructors and Destructors

Types of Constructors:
- Default constructor - automatically provided by the compiler
- Parameterized constructor - accepts parameters
- Copy constructor - copies an existing object
- Static constructor - called only once
- Private constructor - only called within the class

Destructors/finalizers:
```cs
~myClass
```

Method called by the garbage collector prior to removal from memory.

Important Points:
- One destructor per class
- No return type and has the same name as the class name
- Distinguished from the constructor by the Tilde symbol (~)
- No parameters or modifiers
- Can only be used within classes
- Cannot be overloaded or inherited
- Called when the program exits.
- Internally, the destructor calls the finalise method on the base class of an object.

Avoid destructors - they have weird side effects. Use only when resources MUST ABSOLUTELY be freed up.


##### Private Backing Fields

Usually you'd use auto-properties like this:

```cs
public int BirthYear { get; set; } = 2022;

```

But if you need to add ***validation*** or ***logic*** then you need a *private backing field*:

```cs
private int _birthYear = 2022;
public int BirthYear
{
    get => _birthYear;
    set
    {
        if (value > DateTime.Now.Year)
            throw new ArgumentException("Birth year cannot be in the future!");
        _birthYear = value;
    }
}

```

**Why Use Auto-Properties?**

1. **Simplicity**: Removes unnecessary code for backing fields unless required.
2. **Maintainability**: Modern C# developers expect auto-properties where possible.
3. **Performance**: Auto-properties have no performance difference compared to manual backing fields.

**When Should You Use Backing Fields?**

1. If you need validation logic.
2. If the private field stores data different from what is exposed by the property (e.g., caching or computed fields).
3. If you need a different accessibility level (e.g., private setter):
```cs
public int BirthYear { get; private set; } = 2022;
```

>[!info]
>C# automatically creates a private, hidden backing fields for public properties, so OOP principles are still adhered to despite it looking like the field is public. Fields are not the same as properties. Think of the backing field as the actual private property that exists in Java. The public property is a way of providing access to the private data native to the C# language, rather than writing a getter/setter method for each property. Basically, just use public auto-properties in general. The field will still be encapsulated.

Remember you can still make accessors private or protected:

```cs
public int BirthYear { get; private set; }
```

##### **Key Differences Between Java and C#:**

|Aspect|Java|C#|
|---|---|---|
|**Fields**|Typically `private`.|Typically `private`, or implicit via auto-properties.|
|**Properties**|Implemented as `getX` and `setX`.|Built-in support via `public int BirthYear { get; set; }`.|
|**Encapsulation**|Requires explicit methods.|Implicit with auto-properties but still adheres to encapsulation.|
|**Boilerplate**|Requires backing fields manually.|Backing fields are generated automatically when needed.|
 **Should You Use Public Properties Instead of Private Fields?**

- **Yes**, in most cases where:
    
    - There’s no additional logic required for the property.
    - The property is simple and self-contained.
- **No**, if you:
    
    - Need to enforce custom validation or complex logic in the getter/setter.
    - Require stricter control over internal state.

 **Java vs. C#: A Summary for You**

Think of C#’s **auto-properties** as shorthand for Java’s `get/set` methods. While Java requires you to **manually define** fields and methods, C# simplifies this without breaking OOP principles.

In practice:

- Use **auto-properties** when your fields don’t need custom logic.
- Use **private fields with explicit getters/setters** when you need more control, just like in Java.

Does this help clarify the differences?


##### OOP Principles
- _Abstraction_ - Modeling the relevant attributes and interactions of entities as classes to define an abstract representation of a system.
- _Encapsulation_ - Hiding the internal state and functionality of an object and only allowing access through a public set of functions.
- _Inheritance_ - Ability to create new abstractions based on existing abstractions.
- _Polymorphism_ - Ability to implement inherited properties or methods in different ways across multiple abstractions.

Encapsulation - use access modifiers to hide properties and methods:
- Public
- Private - only inside the class
- Protected - class + child classes
- Internal - accessible within the project

## LINQ

LINQ is a way to query data:

```cs
List<int> collection = new List<int> { 1, 2, 3, 4, 5 };

        var evenNumbers = from num in collection
            where num % 2 == 0
            select num;

        Console.WriteLine("Even numbers: " + string.Join(", ", evenNumbers));
```

`from` is a LINQ keyword.

There is also a method-chainable syntax:

```cs
int[] numbers = { 1, 2, 3, 4, 5, 6 };

var evenNumbers = numbers.Where(num => num % 2 == 0);

foreach (var number in evenNumbers)
{
    Console.WriteLine(number);
}
```

Language-Integrated Query can be used on databases, XML, in-memory collections, or remote services.

It's like SQL inside C#.

 **Key Features of LINQ**

1. **Declarative Syntax**: LINQ uses a high-level, SQL-like query syntax that is easy to read and understand.
2. **Consistency**: The same syntax can be used for different data sources (e.g., arrays, lists, XML, SQL databases).
3. **Strong Typing**: Queries are checked at compile-time, reducing runtime errors.
4. **Deferred Execution**: Many LINQ queries are not executed immediately; they are evaluated only when you enumerate over the query results.

LINQ can operate on any data source that implements the `IEnumerable<T>` or `IQueryable<T>` interfaces.

##### Common LINQ Methods

Here are some frequently used LINQ extension methods:

1. **Where**: Filters elements.
    
    ```csharp
    var filtered = list.Where(x => x > 10);
    ```
    
2. **Select**: Projects each element into a new form.
    
    ```csharp
    var squares = list.Select(x => x * x);
    ```
    
3. **OrderBy/OrderByDescending**: Sorts elements.
    
    ```csharp
    var ordered = list.OrderBy(x => x.Name);
    ```
    
4. **GroupBy**: Groups elements by a key.
    
    ```csharp
    var groups = list.GroupBy(x => x.Category);
    ```
    
5. **Aggregate**: Performs an accumulation operation.
    
    ```csharp
    var sum = list.Aggregate((a, b) => a + b);
    ```
    
6. **Join**: Joins two collections based on a key.
    
    ```csharp
    var joined = customers.Join(orders, 
                                 c => c.Id, 
                                 o => o.CustomerId, 
                                 (c, o) => new { c.Name, o.OrderDate });
    ```
    

---

**Benefits of LINQ**

1. **Readability**: Simplifies complex data manipulations with concise and expressive syntax.
2. **Maintainability**: Query logic is embedded directly in the code, reducing dependency on external query languages.
3. **Strongly Typed**: Compile-time checks catch many errors early.
4. **Reusability**: LINQ queries are composable and reusable.

---
 **LINQ Providers**

Different LINQ providers are used for querying various types of data sources:

1. **LINQ to Objects**: Queries in-memory collections like arrays or lists.
2. **LINQ to SQL**: Queries SQL databases.
3. **LINQ to XML**: Manipulates XML documents.
4. **LINQ to Entities (EF)**: Used with Entity Framework to query databases.

---

**Summary**

LINQ is a powerful feature in C# that simplifies data querying across different data sources. It combines flexibility, strong typing, and readability into one consistent framework, making it a cornerstone of modern C# programming.

>[!warning]
>Remember to always loop through the result of a LINQ query in order to display it in the console. You can't directly `Console.Write`!

```cs
List<int> numbers = new List<int>{1, 2, 3, 4, 5};

var oddNumbers = numbers.Where(number => number % 2 != 0).ToList();

foreach (var number in oddNumbers)
{
    Console.Write(number);
}
```

##### LINQ to XML & LINQ to SQL

XML is Extensible Markup Language.
SQL is Structured Query Language.

They both store data in a certain way.

With LINQ you can query these data stores and update them.

Example XML:

```xml
<Library>
	<Book>
		<Title>Introduction to LINQ</Title>
		<Author>John Doe</Author>
		<Genre>Programming</Genre>
	</Book>
		<Book>
		<Title>C# Basics</Title>
		<Author>Imran Afzal</Author>
		<Genre>Programming</Genre>
	</Book>
</Library>
```

 ```cs
XDocument xmldoc = XDocument.Load("library.xml");
var booksByAuthor = from book in xmlDoc.Descendants("Book)
					where book.Element("Author).Value == "John Doe"
					select book
```

Or chainable:

```cs
XDocument xmldoc = XDocument.Load("library.xml");
var booksByAuthor = xmldoc.Descendants("Book")
    .Where(book => book.Element("Author")?.Value == "John Doe");
```

That was XML, now for SQL.

A database is an organised collection of data.

We can organise data in a table of rows and columns.

```cs
var context = new MyDataContext("DatabaseName");

var booksByAuthor = from book in context.Books
					where book.Author == "John Doe"
					select book;

foreach (var book in booksByAuthor) {
	Console.WriteLine($"Title: {book.title}, Author: {book.Author}, Genre: {book.genre}");
}
```

Or chainable:

```cs
var context = new MyDataContext("DatabaseName");

var booksByAuthor = context.Books
    .Where(book => book.Author == "John Doe");

foreach (var book in booksByAuthor)
{
    Console.WriteLine($"Title: {book.Title}, Author: {book.Author}, Genre: {book.Genre}");
}
```

## File I/O and Serialization

##### File I/O

When a file is opened for reading or writing, it becomes a stream.

This process is known as file handling.

The stream is the sequence of bytes moving through the communication path.

Streams represents data as a continuous flow of data (bytes or characters etc.) rather than a fixed size block.

There are two main streams:
- Input stream - used for reading data
- Output stream - used for writing data

System.IO namespace provides the classes for this:
- File Stream - basic file operations in non-text files
- File Reader - character data (text files)
- File Writer - character data 

Creating a text file then reading it:

```cs
string contentToWrite = "Hello World!";
File.WriteAllText("example.txt", contentToWrite);
string contentRead = File.ReadAllText("example.txt");
Console.WriteLine(contentRead);
```

##### Serialization and Deserialization in File Handling

Stores and retrieves complex structures.

Serialization is the process of converting objects into a byte stream to make them suitable for storage in memory, files, or databases.

Deserialization is the reverse process turning a byte stream to an object.

Types of serialization/deserialization:
- Binary - convert objects to binary
- JSON - convert objects to JSON
- XML - convert objects to XML
- Data Contract - provides a clear set of rules for serialisation/deserialisation

Example using a defined type:

```cs
using System;
using System.IO;
using System.Text.Json;

// Define a class for the person object
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
    public string Address { get; set; }
}

class Program
{
    static void Main()
    {
        // Create a new object
        var person = new Person { Name = "John Doe", Age = 25, Address = "123 Main Street" };

        // Serialize the object to a JSON string
        File.WriteAllText("person_data.json", JsonSerializer.Serialize(person));

        // Deserialize the JSON string to a Person object
        var deserializedPerson = JsonSerializer.Deserialize<Person>(File.ReadAllText("person_data.json"));

        // Print the deserialized object
        Console.WriteLine($"Deserialized person: {deserializedPerson.Name}, {deserializedPerson.Age}, {deserializedPerson.Address}");
    }
}
```

Example using a dynamic type by parsing the JsonElement:

```cs
using System;
using System.IO;
using System.Text.Json;

class Program
{
    static void Main()
    {
        // Create a new object with an anonymous type
        var person = new { Name = "John Doe", Age = 25, Address = "123 Main Street" };

        // Serialize the object to a JSON string
        File.WriteAllText("person_data.json", JsonSerializer.Serialize(person));

        // Deserialize the JSON string to a JsonElement
        var deserializedPerson = JsonSerializer.Deserialize<JsonElement>(File.ReadAllText("person_data.json"));

        // Access properties from the JsonElement
        string name = deserializedPerson.GetProperty("Name").GetString();
        int age = deserializedPerson.GetProperty("Age").GetInt32();
        string address = deserializedPerson.GetProperty("Address").GetString();

        // Print the deserialized object
        Console.WriteLine($"Deserialized person: {name}, {age}, {address}");
    }
}

```

## Asynchronous Programming and Threading

##### Threads & Task Parallel Library (TPL)

A thread is a single sequence of instructions that a process can execute.

For example if you need to fetch a large amount of data, you can divide the entire process into manageable units.

Each unit is referred to as a thread.

Each thread operates independently, allowing multiple threads to execute simultaneously.

This approach improves efficiency as different threads can work on different parts of the data at the same time, making it a better use of the available resources and potentially speeding up execution.

Threads are a lightweight process.

A common use of threads is the implementation of concurrent programming by modern operating systems - this saves wastage of the CPU cycle and increases the efficiency of an application.

The life cycle of a thread starts when an object of the System.Threading.Thread class is created.

It ends when the thread is terminated or completed execution.

There can be:
- Single-threading - one sequence of instructions at a time
- Multi-threading - concurrent execution of multiple threads

Task Parallel Library is a set of APIs that simplifies the development of parallel and concurrent code.

The TPL uses threads to make things happen at the same time whilst hiding the complicated details.

Devs tell TPL what tasks they want done and the TPL figures out how to use threads effectively to get those tasks done simultaneously.

TPL does behind the scenes work - makes multithreading easier for the dev.
##### Async and Await for Asynchronous Programming

Asynchronous programming is a way of organising tasks independently from the main programme flow, allowing them to run concurrently and without blocking other operations.

This is unlike synchronous programming where tasks are executed sequentially.

It prevents a programme from freezing during pauses caused by operations such as Web API calls.

Async/Await gives the syntax for doing asynchronous programming in C#.

Async means don't wait for a task to finish before continuing with other work.

Await means pause for a task to finish before proceeding to the next task.

Example of Async Code:

```cs
namespace Thread
{
    internal class Student
    {
        public int ID { get; set; }
        public string Name { get; set; }
    }
}

namespace Thread
{
    internal class Program
    {
        static List<Student> GetStudentData() => Enumerable.Range(1, 10).Select(i => new Student
        {
            ID = i,
            Name = $"Student {i}"
        }).ToList();

        static async Task<int> ProcessStudentAsync(Student student)
        {
            await Task.Delay(100);
            Console.WriteLine($"Processing data for {student.Name}");
            return student.ID * 2;
        }

        static async Task Main()
        {
            var students = GetStudentData();
            var results = await Task.WhenAll(students.Select(ProcessStudentAsync));
            Console.WriteLine($"Total: {results.Sum()}");
        }
    }
}
```

Explanation:

Async/await is used to handle long-running tasks.

- The `async` keyword :
1. Marks the method as being asynchronous
2. Method able to contain `await` statements
3. Returns a `Task`/`Task<T>` (for asynchronous operations) or `void` (for event handlers)

- `await`:
1. Pauses execution until task is complete
2. Frees up the thread to handle other operations during the wait.

**How It Works**
- When you use `await`, the method **does not block** the thread. Instead, it registers a continuation that resumes the method's execution after the awaited task completes.
- The `async` keyword is required on the method to allow `await` to be used inside it.

```cs
using System;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
        Console.WriteLine("Fetching data...");
        string result = await FetchDataAsync();
        Console.WriteLine($"Result: {result}");
    }

    static async Task<string> FetchDataAsync()
    {
        // Simulate a delay
        await Task.Delay(2000); // Wait for 2 seconds (non-blocking)
        return "Data retrieved!";
    }
}
```

**Explanation:**

1. `FetchDataAsync` is marked `async` and returns a `Task<string>`.
2. Inside `FetchDataAsync`, `await Task.Delay(2000)` simulates a 2-second delay.
3. The `await` keyword makes the method return control to the caller (`Main`) while waiting.
4. Once the delay completes, the program resumes from where it left off and returns the result.

API Example:

```cs
using System;
using System.Net.Http;
using System.Threading.Tasks;

class Program
{
    static async Task Main(string[] args)
    {
        Console.WriteLine("Starting API call...");
        string data = await GetApiDataAsync();
        Console.WriteLine($"API Data: {data}");
    }

    static async Task<string> GetApiDataAsync()
    {
        using (HttpClient client = new HttpClient())
        {
            HttpResponseMessage response = await client.GetAsync("https://jsonplaceholder.typicode.com/posts/1");
            response.EnsureSuccessStatusCode();
            string responseBody = await response.Content.ReadAsStringAsync();
            return responseBody;
        }
    }
}

```

Basically:
- Mark the method with `async`
- Get it to return a `Task` or `Task<T>`
- Use `await` int the method body

##### Managing Concurrent Processes (Locks)

One way to manage concurrent processes is to use async/await.

Another approach is - synchronisation.

Synchronisation mechanisms:
- Locks
- Semaphores
- Mutexes

These tools help enforce mutual exclusion meaning that only one process can access a shared resource at any given time.

A lock might ensure that when once process is modifying a shared variable, other processes must wait until the modification is complete.

**Locks in C#:**

```cs
namespace SpeechCompetition
{
    internal class Program
    {
        static int participantCount = 0;

        static void Main(string[] args)
        {
            EnterSpeechCompetition(1);
        }

        static void EnterSpeechCompetition(object participantId)
        {
            Monitor.Enter(typeof(Program));
            try{
                participantCount++;
                Console.WriteLine($"Participant {participantId} entered the competition. Total participants: {participantCount}");
            }
            finally{
                Monitor.Exit(typeof(Program));
            }
        }
        
    }
}
```

**Explanation**
1. `participantCount` is a shared resource When accessed by multiple threads, without proper synchronization, it could lead to race conditions (undesired effect).
2. The `EnterSpeechCompetition` method uses the `Monitor` class to ensure that only one thread can increment `participantCount` and print the output at a time.
3. `Monitor.Enter(typeof(Program))`locks the Program class
4. `Monitor.Exit` releases the lock ALWAYS (due to it being in a finally block) - this is crucial in case an issue causes the lock to hold indefinitely.

**Explanation of the typeof part:**
- `typeof(Program)` retrieves the **type object** for the `Program` class. This is essentially a unique representation of the `Program` class in memory.
- In C#, every class or type has a corresponding type object created by the runtime, which acts as a **global object** for that class.
- In the above code, the `typeof(Program)` is being used as a **lock object**. This means that all threads trying to execute code synchronized on `typeof(Program)` will share the same lock, ensuring only one thread can enter the critical section at a time.
- Essentially any code in the Program class has the same lock, so only one thread can enter the locked section at any one point.

 **Why Use `typeof(Program)` as a Lock?**

- **Shared Resource**: The `participantCount` variable is a shared resource, meaning multiple threads might try to access and modify it simultaneously.
- **Global Lock Object**: `typeof(Program)` ensures that all threads that use this lock are coordinated, regardless of where or how the `Program` class is instantiated. Since `typeof(Program)` always refers to the same object, it serves as a reliable, **static-level** lock.

**You could also lock like this, by creating a separate static object to serve as the lock:**

```cs
private static readonly object lockObj = new object();

static void EnterSpeechCompetition(object participantId)
{
    lock (lockObj) // Equivalent to Monitor.Enter(lockObj) and Monitor.Exit(lockObj)
    {
        participantCount++;
        Console.WriteLine($"Participant {participantId} entered the competition. Total participants: {participantCount}");
    }
}
```

**Why Use `typeof(Program)` Instead of a Separate Lock Object?**

1. **Simplicity**:
    - If your critical section involves shared resources tied to the type itself (like static fields), `typeof(Program)` is a convenient choice.
2. **Avoids Additional Object Creation**:
    - You don’t need to define and manage an extra static object (like `lockObj`).
    - 

**Behavior**

- When multiple threads call `EnterSpeechCompetition`:
    1. The first thread acquires the lock (`Monitor.Enter`).
    2. Other threads attempting to enter the critical section are blocked until the first thread calls `Monitor.Exit`.
    3. Once the lock is released, the next waiting thread acquires the lock and enters the critical section.

`Monitor.Enter` and `Monitor.Exit` can also be done using the `lock` keyword which is syntactic sugar for the other two. But use `Monitor` gives more control.


**Easy way to lock a resource:**

```cs
private static readonly object lockObj = new object();

lock (lockObj)
{
    participantCount++; // Critical section
}
```

>[!Further Info]
>The lock object itself does not store the information about whether the shared resource is being used or not. The CLR—Common Language Runtime in .NET maintains this information in its internal data structures.

**How Locking Works**
1. You create/use a lock object
2. The runtime uses the memory address of the object as a unique identifier for synchronisation (the key)
3. The runtime keeps track of whether the lock is "held" (owned by a thread) in an internal structure associated with the lock object

This metadata includes:
- Which thread currently owns the lock.
- How many times the lock has been acquired (useful for **reentrant locks**, where the same thread can re-acquire the lock without deadlocking itself).
- A queue of other threads waiting to acquire the lock.

**Where is the lock state stored?**

The lock state is managed by the **runtime's internal synchronization mechanism**, not the object itself. Here’s how it works:

*Lock Metadata:*
- Every object in .NET has a small overhead (separate from its actual fields and data) called a sync block index, part of the object header in memory.
- This sync block index is a pointer to a sync block table maintained by the CLR.
- When a thread locks an object:
		- The runtime uses the sync block index to look up the sync block table and find or create the synchronization metadata for that object.

*Sync Block Table:*
- The sync block table is a system-wide data structure in the CLR that tracks locks, thread ownership, and waiting threads for all objects used as lock anchors.
- This table contains:
		- Whether the lock is currently held.
		- Which thread owns the lock.
		- Information about threads waiting to acquire the lock.

 **Key Points About the Lock Object**

- The lock object itself doesn’t store any special data. It’s simply an ordinary object.
- The **runtime manages synchronization metadata** in a separate internal structure (sync block table), associated with the object’s header.
- The lock object's memory address serves as the "key" to access its sync block metadata.

**Example Workflow of a Lock**

For `lock(lockObj)`:

1. The runtime calculates the memory address of `lockObj`.
2. It checks the sync block metadata associated with `lockObj`:
    - If the lock is **free**, the runtime updates the metadata to mark it as "owned" by the current thread.
    - If the lock is **already owned**, the runtime blocks the current thread and adds it to a queue of waiting threads in the metadata.
3. When `Monitor.Exit(lockObj)` is called (or the `lock` block ends), the runtime releases the lock:
    - If other threads are waiting, one of them is granted ownership of the lock.
 
 **Why Not Store Lock State in the Lock Object Itself?**

1. **Separation of Concerns**:
    - Keeping lock state in a separate system-level structure (the sync block table) ensures that locking doesn’t interfere with the object’s primary purpose (holding data or representing a resource).
2. **Efficiency**:
    - By centralizing lock management in the runtime, the CLR can optimize synchronization, like reducing memory overhead for objects that aren’t used as locks.
3. **Thread Safety**:
    - The runtime ensures thread-safe access to the sync block table, avoiding corruption or race conditions.

**Summary**

- The lock object (`lockObj`) serves as a **unique identifier** for the runtime to manage thread synchronization.
- The runtime maintains the lock state (e.g., whether it’s in use, which thread owns it) in a **sync block table**, not in the object itself.
- This design keeps locking lightweight for objects that aren’t used as locks and ensures robust synchronization for objects that are.

**How to choose a lock:**
- It should be ***unique*** - and the threads should use the same lock object (in order for the runtime to treat them as synchronised)
- ***Scoped*** appropriately - `static readonly object` or `typeof(SomeClass)` because all threads across all instances of the class need to share the lock.
- Only use a non-static lock object if each instance of the class can operate independently
- ***Visibility*** - should not be accessible outside the class or method where it's used as that might cause other parts of the programme or other threads to inadvertently lock on the same object for unrelated purposes, leading to unintended behaviour or deadlocks

>[!warning]
>Do not use a `public` lock! Any external code could potentially lock on it and intefere with your logic.

**Why did we use typeof(Program) in the first code example?**

In the original code:

```cs
Monitor.Enter(typeof(Program));
```

- The lock object is typeof(Program), which represents the type Program at runtime.
- This works because typeof(Program) is unique and static, making it suitable for protecting shared static data like participantCount.

However:

- It’s arbitrary in the sense that any other static, unique object would work equally well (e.g., private static readonly object lockObj = new object();).
- Using typeof(Program) is a matter of convenience—it’s readily available and avoids creating an extra static object just for locking.

**Best Practice:**
1. Use a dedicated lock object where possible:
```cs
private static readonly object participantLock = new object();
```

2. Avoid using Public or Common object such as `this`, string constants, or widely accessible static objects:
```cs
lock ("shared-lock") { ... }
```

>[!warning]
>DO NOT USE THE ABOVE!

3. Match lock scope to resource scope - use static locks for static resources and instance locks for instance-level resources.

4. Avoid overusing `typeof(Class)` - can be misleading because it doesn't explicitly convey that it's being used as a synchronisation anchor. A dedicated lock object is clearer.

Conclusion about the original lock code:

- `typeof(Program)` works fine because `participantCount` is static
- The lock is unique 
- However it's a bit arbitrary and using a purpose-built static object would be a more conventional and clearer choice, especially in larger, more complex programmes:
```cs
private static readonly object participantLock = new object();
```

##### Threads In Use

```cs
Threat t = new Thread(InsertMethodHere)
t.start()

// You can add arguments to the thread, which will be passed to the method
```

Example in use with locking:

```cs
using System;
using System.Threading;

namespace ThreadLockExample
{
    internal class Program
    {
        private static int counter = 0; // Shared resource
        private static readonly object counterLock = new object(); // Lock object

        static void Main(string[] args)
        {
            // Create multiple threads
            Thread thread1 = new Thread(IncrementCounter);
            Thread thread2 = new Thread(IncrementCounter);
            Thread thread3 = new Thread(IncrementCounter);

            // Start threads
            thread1.Start();
            thread2.Start();
            thread3.Start();

            // Wait for all threads to finish
            thread1.Join();
            thread2.Join();
            thread3.Join();

            // Final counter value
            Console.WriteLine($"Final counter value: {counter}");
        }

        private static void IncrementCounter()
        {
            for (int i = 0; i < 10; i++)
            {
                // Lock the critical section to ensure thread safety
                lock (counterLock)
                {
                    counter++;
                    Console.WriteLine($"Thread {Thread.CurrentThread.ManagedThreadId} incremented counter to {counter}");
                }

                // Simulate some work with a small delay
                Thread.Sleep(10);
            }
        }
    }
}
```

The `.Join()` instance method of the `Thread` class is used to block the current thread using it (e.g. the Main thread) and wait until the thread being joined has finished executing.

This ensures proper synchronisation.

Join supports an overload allowing you to specify a timeout - if the join doesn't finish in the allotted time, the method returns false:

```cs
thread.Join(5000);
```

If you don't use`Join()`, the Main thread will go ahead and execute whatever tasks it contains before the other threads complete.

**Best Practices**
- Avoid Excessive Blocking - using Join can lead to the main thread or other threads being unnecessarily blocked. Ensure you actually need to wait for the worker thread.
- Handle Timeouts - Always consider whether a timeout is appropriate. Long waits can make your application unresponsive.
- Use Alternatives if Possible - For more complex synchronization scenarios, consider using higher-level concurrency primitives like Task, Task.Wait, or async/await.