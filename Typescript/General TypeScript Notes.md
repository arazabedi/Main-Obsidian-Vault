#TypeScript

- Use interfaces for defining object types, and type aliases for more complex types.
- It's very common to use the **[[#Spread Operator]]** (`...`) to split an enumerable (array/object/string) into positional arguments (this is also in JS).
- **Generic functions** are the ones with angle brackets that let you define the type of the argument when calling the function.
- Angle brackets denote generic types. For example, `<T>` denotes the generic type parameter T which can be any type. See [[#Generics]]
- https://basarat.gitbook.io/typescript/ (Great TS book).
- Always read properly as it's easy to miss things in TypeScript (e.g. `String[]` looks like `String` but is actually string array)
- Use **[[#Union Types]]** when want to create variables that contain objects of multiple different types. For example when a Person can be either a User or an Admin, make interfaces for both, and make Person a type alias that equals either User or Admin (`User | Admin`) 
- Use the `in` operator to see if a property is in an object (useful when writing functions that adhere to different interfaces)
- To 'narrow down' a function argument to a specific type, use [[#Type Predicates]].
- Use the `keyof` type operator to take an object and produce a string or numeric literal union of its keys
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

Use generics when you want a function, class, or interface to be able to take any type as parameters (i.e. for functions: arguments, classes: constructor arguments, interfaces: properties). They are useful when the type of a parameter doesn't matter, but you don't want to use the `any` type as that defeats the point of TypeScript. You can also make

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

- Interface requires that an object of type Pair must contain two parameters of type T and U respectively (basically any type you want)
- Any variable containing an object that matches this description is considered a `Pair`

4. Generic Type Aliases

```ts
type Pair<T, U> = {
  first: T;
  second: U;
};

const pair: Pair<string, number> = { first: "hello", second: 42 };
```

```ts
type Result<T> = { success: true; value: T } | { success: false; error: string };

const successResult: Result<number> = { success: true, value: 42 };
const errorResult: Result<number> = { success: false, error: "Something went wrong" };

```

- Same as generic interfaces but with the flexibility to use intersection (`&`) and union (`|`) types


### Union Types

```ts
interface User {
    name: string;
    age: number;
    occupation: string;
}

interface Admin {
    name: string;
    age: number;
    role: string;
}

export type Person = User | Admin;
```

### Type Predicates

```ts
function isFish(pet: Fish | Bird): pet is Fish {
  return (pet as Fish).swim !== undefined;
}
```

The return type for the above function is a type predicate.

`pet is Fish` is our type predicate in this example. A predicate takes the form `parameterName is Type`, where `parameterName` must be the name of a parameter from the current function signature.

Any time `isFish` is called with some variable, TypeScript will _narrow_ that variable to that specific type if the original type is compatible.
### `Keyof` Operator

```ts
type Point = { x: number; y: number };
type P = keyof Point;
```

Same as:

```ts
type P = "x" | "y"
```

So basically use `keyof` to make union types from the keys of object. *Very* useful for making flexible components such as classes ([[4. Generics#CSV Writer Project|See Generic Classes in CSV Writer Project]])

OR

```ts
export class CSVWriter<T> {
	constructor(private columns: (keyof T)[]) {
		this.csv = this.columns.join(',') + '\n'
	}

//...

}
```

