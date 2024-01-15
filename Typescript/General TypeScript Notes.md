#TypeScript
- Use interfaces for defining object types, and type aliases for more complex types.
- It's very common to use the **[[#Spread Operator]]** (`...`) to split an enumerable (array/object/string) into positional arguments (this is also in JS).
- **Generic functions** are the ones with angle brackets that let you define the type of the argument when calling the function.
- Angle brackets denote generic types. For example, `<T>` denotes the generic type parameter T which can be any type. See [[#Generics]]
- https://basarat.gitbook.io/typescript/ (Great TS book).

### Spread Operator

#SpreadOperator

Uses:

1. **Merge array**

```ts
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

const combinedArray = [...arr1, ...arr2];
// combinedArray is [1, 2, 3, 4, 5, 6]
```

2. **Merge object**

```ts
const obj1 = { a: 1, b: 2 };
const obj2 = { c: 3, d: 4 };

const combinedObject = { ...obj1, ...obj2 };
// combinedObject is { a: 1, b: 2, c: 3, d: 4 }
```

3. Pass elements of array as arguments to function

```ts
function sum(a, b, c) {
  return a + b + c;
}

const numbers = [1, 2, 3];
const result = sum(...numbers);
// result is 6
```

4. String to array

```ts
const str = "hello";
const chars = [...str];
// chars is ['h', 'e', 'l', 'l', 'o']
```



### Generics

#Generics

Use generics when you want a function, class, or interface to be able to take any type as parameters (i.e. for functions: arguments, classes: constructor arguments, interfaces: properties). They are useful when the type of a parameter doesn't matter, but you don't want to use the `any` type as that defeats the point of TypeScript.

Examples:

1. **Generic Functions:**

```ts
function identity<T>(value: T): T {
  return value;
}

const result: number = identity<number>(42);
```

- Angle brackets declare and specify generic type parameters in functions. 
- Generics allow you to write functions that operate on values of any type.

2. **Generic Classes:**

```ts
class Box<T> {
  private value: T;

  constructor(value: T) {
    this.value = value;
  }

  getValue(): T {
    return this.value;
  }
}

const boxOfNumbers = new Box<number>(42);
const boxedNumber: number = boxOfNumbers.getValue();
```

- Angle brackets declare and specify generic type parameters.
- This allows you to create classes that work with different types.

3. **Generic Interfaces:**

```ts
interface Pair<T, U> {
  first: T;
  second: U;
}

const pair: Pair<string, number> = { first: "hello", second: 42 };
```