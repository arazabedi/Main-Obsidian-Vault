#Java
## Introduction to Hash Maps
___
A `HashMap` is one of Java's most widely used pre-built data structures.

It is used whenever data is stored as key-value pairs where values can be added, retrieved, and deleted using keys.

```java
HashMap<String, String> postalCodes = new HashMap<>();
postalCodes.put("00710", "Helsinki");
postalCodes.put("90014", "Oulu");
postalCodes.put("33720", "Tampere");
postalCodes.put("33014", "Tampere");

System.out.println(postalCodes.get("00710"));
// Output: Helsinki
```


If the hash map doesn't contain the key then it will return a `null` reference.

```java
HashMap<String, String> numbers = new HashMap<>();
numbers.put("One", "Uno");
numbers.put("Two", "Dos");

String translation = numbers.get("One");
System.out.println(translation);

System.out.println(numbers.get("Two"));
System.out.println(numbers.get("Three"));
System.out.println(numbers.get("Uno"));
// Output:
// Uno 
// Dos 
// null 
// null
```

You must import the utility:

```java
import java.util.HashMap;
```

Hash Map Overwrite
___
If you put a new value in with a key that already exists, the old value will be overwritten.

```java
HashMap<String, String> numbers = new HashMap<>();
numbers.put("Uno", "One");
numbers.put("Dos", "Zwei");
numbers.put("Uno", "Ein");

String translation = numbers.get("Uno");
System.out.println(translation);

System.out.println(numbers.get("Dos"));
System.out.println(numbers.get("Tres"));
System.out.println(numbers.get("Uno"));
```

```
Ein 
Zwei 
null 
Ein
```

The `containsKey` method
___
Use this to check the existence of a key in a hash map.

Going through a hash map's keys
___
If you want to search through a map's keys, you have to use the `keySet()` method and loop through it using `for`.

```java
public ArrayList<Book> getBookByPart(String titlePart) {
    titlePart = sanitizedString(titlePart);

    ArrayList<Book> books = new ArrayList<>();

    for(String bookTitle : this.directory.keySet()) {
        if(!bookTitle.contains(titlePart)) {
            continue;
        }

        // if the key contains the given string
        // we retrieve the value related to it
        // and add it tot the set of books to be returned

        books.add(this.directory.get(bookTitle));
    }

    return books;
}
```

This way we lose the speed advantage of the hash map.

Going through a hash map's values using `values()`
___
```java
public ArrayList<Book> getBookByPart(String titlePart) {
    titlePart = sanitizedString(titlePart);

    ArrayList<Book> books = new ArrayList<>();

    for(Book book : this.directory.values()) {
        if(!book.getName().contains(titlePart)) {
            continue;
        }

        books.add(book);
    }

    return books;
}
```

You can get all the values of the hash map using `values`.

Primitive variables in a hash map
___
Hash maps only take reference variables.

Java converts primitives to their corresponding reference types when using any of Java's built in data structures (such as ArrayList and HashMap).

Although the value `1` can be represented as a value of the primitive `int` variable, its type should be defined as `Integer` when using ArrayLists and HashMaps.

```java
HashMap<Integer, String> hashmap = new HashMap<>(); // works
hashmap.put(1, "Ole!");
HashMap<int, String> map2 = new HashMap<>(); // doesn't work
```

Both the keys and objects are always reference variables.

```java
int -> Integer
double -> Double
char -> Character
```

The automatic conversion of primitives to reference types is called auto-boxing in Java.

The automatic conversion is also possible in the other direction:

```java
int key = 2;
HashMap<Integer, Integer> hashmap = new HashMap<>();
hashmap.put(key, 10);
int value = hashmap.get(key);
System.out.println(value);
```

```java
10
```

There is some risk in type conversions.

Converting a `null` to an `int` will yield an error:

```java
java.lang.reflect.InvocationTargetException
```

Therefore if you use `hashmap.get()` with a key that doesn't exist, then make a new `int` variable to store it, you will get the above error.

Ensure that the converted value is not `null`.

The `getOrDefault(key, default)` method
___
By using this method, you will return the value of the key given as a parameter, or if the key is not found, the value of the second parameter passed to it.

These two snippets have the same functionality:

```java
public int timesSighted(String sighted) {
    return this.allSightings.getOrDefault(sighted, 0);
}
```

```java
public int timesSighted(String sighted) {
    if (this.allSightings.containsKey(sighted)) {
        return this.allSightings.get(sighted);
    }

    return 0;
}
```


## Similarity of Objects
___
`equals()` returns true if the parameter has the same reference as the object it is called on.

Therefore, two objects with identical instance variables are not considered equal by the `equals()` method.

However, some classes, such as `String`, have overwritten Java's default implementation of the `equals()` method, such that it will return true if two strings with identical content are compared.

If you wanted a `Book` class that treats any `Book` object with identical instance variables to another `Book` object as 'equal', you must write your own `equals()` method.

This can be long.

```java
public boolean equals(Object comparedObject) {
    // if the variables are located in the same place, they're the same
    if (this == comparedObject) {
        return true;
    }

   // if comparedObject is not of type Book, the objects aren't the same
   if (!(comparedObject instanceof Book)) {
        return false;
    }

    // let's convert the object to a Book-olioksi
    Book comparedBook = (Book) comparedObject;

    // if the instance variables of the objects are the same, so are the objects
    if (this.name.equals(comparedBook.name) &&
        this.published == comparedBook.published &&
        this.content.equals(comparedBook.content)) {
        return true;
    }

    // otherwise, the objects aren't the same
    return false;
}
```

The reliance on default methods such as `equals` is why Java demands that only reference type variables be added to `ArrayList` and `HashMap`.

Primitive variables don't have such default methods.

Approximate Comparison With HashMap
___
You can use the `hashcode` method to compare objects (approximately).

Call `hashcode()` on an object to create a hash code from it that tells us a bit about the object's content.

If two objects have the same hash code, they may be equal.

If two objects have different hash codes, they are always unequal.

`hashCode` is a method written for the `String` and `Integer` classes, but if you want to use something else as a key in a hash map, you may have to define a hash code method yourself.

This is because `HashMap` uses hash codes.

ChatGPT's explanation of this part of the chapter:

Java classes need to implement the `hashCode()` method when their instances are intended to be used as keys in hash maps because the `hashCode()` method is used to determine the bucket location in the hash table where the key-value pair will be stored. Here's why:

1. Hashing and Hash Maps: Hashing is a technique used to efficiently store and retrieve data in a data structure called a hash table or hash map. A hash map uses an array of buckets, and each bucket can hold multiple key-value pairs. When you store a key-value pair in a hash map, the key is hashed to generate an index, which is the bucket location where the pair will be stored.
    
2. Hash Code: The `hashCode()` method is a method defined in the `Object` class, which is the root class for all Java classes. It returns an integer value that represents the hash code of an object. The hash code is typically calculated based on the object's fields and properties. The `hashCode()` method is used in conjunction with the `equals()` method for various Java APIs, such as hash-based collections like hash maps and hash sets.
    
3. Hash Map Internals: When you add a key-value pair to a hash map, the hash code of the key is calculated using the `hashCode()` method. The hash code is then used to determine the bucket where the key-value pair should be stored. Different keys may produce the same hash code, resulting in what is known as a hash collision. In such cases, the hash map uses additional techniques, such as chaining or probing, to handle collisions and store multiple entries in the same bucket.
    
4. Key Equality: The `hashCode()` method is crucial for proper functioning of hash maps because it is used to determine the bucket location where the key-value pair should be stored, and later to locate the pair when you try to retrieve it. However, it's important to note that the `hashCode()` method alone is not sufficient to ensure proper key equality. To achieve that, you also need to override the `equals()` method in your class to provide a custom implementation of key comparison.
    
5. Consistency: According to the general contract of the `hashCode()` method, if two objects are equal based on the `equals()` method, then their hash codes must also be equal. This ensures consistency in the hash map operations and helps in retrieving the correct key-value pair.
    

By implementing the `hashCode()` method in your class, you provide a way to generate a unique hash code for each instance of the class. This allows hash maps to distribute the key-value pairs evenly across the buckets, minimizing collisions and optimizing the performance of key-based operations like insertion, retrieval, and deletion.

**USE POINT 5!**

You can use the hash code method of another variable that has a well functioning hash code method (any variable of a reference type such as  `String` or `Integer`):

```java
public int hashCode() {
    return this.name.hashCode();
}
```

This would however give us a `NullPointerException` error if the name variable is null, therefore we can alter our method:

```java
public int hashCode() {
    if (this.name == null) {
        return this.published;
    }

    return this.name.hashCode();
}
```

Or even better:

```java
public int hashCode() {
    if (this.name == null) {
        return this.published;
    }

    return this.published + this.name.hashCode();
}
```

Now we can use the book as the hash map's key.

> [!summary] 
> **Let's review the ideas once more:** for a class to be used as a HashMap's key, we need to define for it:
> 
> - the `equals` method, so that all equal or approximately equal objects cause the comparison to return true and all false for all the rest
> - the `hashCode` method, so that as few objects as possible end up with the same hash value


The object hash method
___
```java
@Override
public int hashCode() {
	return Objects.hash(liNumber, country);
}
```

Use this to make a hash. Also, see below for more information:

https://www.baeldung.com/java-objects-hash-vs-objects-hashcode


## Grouping Data using Hash Maps
___
If we want to store multiple values per hash map key, we can use lists:

```java
HashMap<String, ArrayList<String>> phoneNumbers = new HashMap<>();
```

Each key added would have a list value.