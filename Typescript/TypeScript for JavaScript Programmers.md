#TypeScript

All JavaScript is TypeScript.

TypeScript adds a number of features which are used to enforce type safety at compile time, at which your code is compiled to normal JavaScript.

The TypeScript compiler - and ESLint if you're using VSCode - will flag up type errors that will otherwise go unnoticed if you use JavaScript.

This means that you have to do less console logging to find out why your code is not working.

##### Types by Inference
___
```typescript
let helloWorld = "Hello World"
```

TypeScript will automatically assign a type if, like in the case above, a variable is assigned a particular value.

##### Defining Types
___
An interface in TS is a structure used for type-checking:

```ts
interface User {
name: string;
id: number;
}

const user: User = {
name: "Hayes",
id: 0,
}
```

If you give any other properties (e.g. `username`) to a User object, the linter will flag this and the compiler will throw an error:

![[Pasted image 20230405163111.png]]

TypeScript supports OOP, just like JavaScript.

You can also use interfaces with classes:

```ts
interface User {
  name: string;
  id: number;
}
 
class UserAccount {
  name: string;
  id: number;
 
  constructor(name: string, id: number) {
    this.name = name;
    this.id = id;
  }
}
 
const user: User = new UserAccount("Murphy", 1);
```

The interface declaration is used for type-safety purposes, while the class declaration is used for the construction of the blueprint for class instances (which take all the OOP principles).

Both Classes and Interfaces define custom types, but they have a few differences:

1. An interface is a type definition that can be used to describe the shape of an object, while a class is a blueprint for creating objects with methods and properties.
2. Interfaces are used for defining the structure of data, while classes are used for creating objects with behavior.
3. Interfaces cannot be instantiated, while classes can be used to create new objects.
4. Interfaces can be implemented by classes, but they cannot be extended like classes.
5. Interfaces are typically used for describing data structures or contracts between parts of a system, while classes are used for encapsulating behavior and state.

You can use interfaces to annotate function parameters and return values to functions: 

```ts
function getAdminUser(): User {
  //...
}
 
function deleteUser(user: User) {
  // ...
}
```

Here we are saying that the argument given to the `deleteUser` function must be of type User.

TypeScript contains all the primitive types available in JavaScript, as well as:

1. any - anything
2. unknown - the type must be declared
3. never - this type can't happen
4. void - a function which returns undefined or has no return value

##### Composing Types
___
You can compose types (add them together).

You can do this with unions, or with generics.

**Unions** - You can declare that a type can be oen of many types.

This is useful for describing the set of string or number literals that a value is allowed to be.

```ts
type MyBool = true | false;

type WindowStates = "open" | "closed" | "minimized";

type LockStates = "locked" | "unlocked";

type PositiveOddNumbersUnderTen = 1 | 3 | 5 | 7 | 9;
```

Unions can also handle different types, for example when a function takes an array or string:

```ts
unction getLength(obj: string | string[]) {
  return obj.length;
}
```

You can use typeof to learn the type of a variable.

```ts
  if (typeof obj === "string") {
    return [obj];
            
(parameter) obj: string
  }
  return obj;
}
```

**Generics** - generics are basically single tags that tell the TS linter what type is allowed in another type.

```ts
type StringArray = Array<string>;
type NumberArray = Array<number>;
type ObjectWithNameArray = Array<{ name: string }>;

const something: ObjectWithNameArray = [{ name: "Araz" }];
console.log(something);
// Output: Araz
```

You can declare new types that use generics:

```ts
interface Backpack<Type> {
  add: (obj: Type) => void;
  get: () => Type;
}
```