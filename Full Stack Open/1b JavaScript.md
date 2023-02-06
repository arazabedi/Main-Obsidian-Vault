Assignment
___
Use `let` and `const` instead of `var`.

`const` variables cannot be re-assigned, but you can push data to a a `cont` array because that is not re-assignment.

Arrays
___
Use `.concat()` instead of `.push()` in React as we want to be more functional.

`.concat()` merges two arrays and returns a new array

```js
const array1 = ['a', 'b', 'c'];
const array2 = ['d', 'e', 'f'];
const array3 = array1.concat(array2);

console.log(array3);
// Expected output: Array ["a", "b", "c", "d", "e", "f"]
```

>[!tip]
>`.concat()` will treat non-array data as an array, so you can use it to add jut one piece of data to an array.

>[!tip]
>`.map()` is very useful for taking a bunch of data and putting html tags around it.

>[!example]
>```js
const m2 = t.map(value => '<li>' + value + '</li>')
console.log(m2)  
// [ '<li>1</li>', '<li>2</li>', '<li>3</li>' ] is printed

Destructing Assignment
___
Quick way to get data from an array or object:

```js
const t = [1, 2, 3, 4, 5]

const [first, second, ...rest] = t

console.log(first, second)  // 1, 2 is printed
console.log(rest)          // [3, 4, 5] is printed
```

1. Chuck your data into an array or an object and assign it to a variable (`t` in the above example)
2. Create a new variable, and give it an array as a name
3. Within the array, add the names that you want to give each data point from the origina array (`t`) - in sequential order
4. Assign this new variable to the original variable
5. Congrats you have destructed your array!

Objects
___
*You can define objects using `object literals`*

```js
const object1 = {
  name: 'Arto Hellas',
  age: 35,
  education: 'PhD',
}

const object2 = {
  name: 'Full Stack web application development',
  level: 'intermediate studies',
  size: 5,
}

const object3 = {
  name: {
    first: 'Dan',
    last: 'Abramov',
  },
  grades: [2, 3, 5, 3],
  department: 'Stanford University',
}
```

*You can reference the properties of objects using 'dot' notation or square brackets:*

```js
console.log(object1.name)         // Arto Hellas is printed
const fieldName = 'age' 
console.log(object1[fieldName])    // 35 is printed
```

*You can add properties with dot notation or square brackets:*

```js
object1.address = 'Helsinki'
object1['secret number'] = 12341
```

*The latter has to be done using brackets because there can be no spaces when using dot notation*

Objects can also have methods of their own, but this is beyond the scope of the Full Stack Open course.

JavaScript also allows you to define objects using constrcutor functions which is similar to classes in other programming languages. However there are slight differences. There is a class syntax which has been added in ES6 but it isn't identical in operation to something like Java's classes.

Arrow Functions
___
Define an Arrow Function:
```js
const sum = (p1, p2) => {
  console.log(p1)
  console.log(p2)
  return p1 + p2
}
```

You can exclude the parentheses if there is a single parameter:
```js
const square = p => {
  console.log(p)
  return p * p
}
```

You can ommit the braces if the function only contains a single expression:
```js
const square = p => p * p
```

When using a map method (or other array manipulation method):
```js
const t = [1, 2, 3]
const tSquared = t.map(p => p * p)
// tSquared is now [1, 4, 9]
```

Call a Function:
```js
const result = sum(1, 5)
console.log(result)
```

Function Declarations
___
This method was used prior to ES6:

```js
function product(a, b) {
  return a * b
}

const result = product(2, 6)
// result is now 12
```

Function Expression
___
With this method, there is no need to give the function a name and the definition 

```js
const average = function(a, b) {
  return (a + b) / 2
}

const result = average(2, 5)
// result is now 3.5
```

In this course everything will be done using arrow functions.

>[!warning]
>`props` is a *fucking* object. It is not the actual thing that you insert into the return (e.g. an array). To get your array, you need to go `props.arrayName`

Object methods and `this`
___
By using React Hooks, we don't need to define objects with methods.

It's good to know though. Older versions of React might need you to do this.

We can assign methods to an object by defining properties that are functions:

```js
const arto = {
  name: 'Arto Hellas',
  age: 35,
  education: 'PhD',
  greet: function() {    console.log('hello, my name is ' + this.name)  },}

arto.greet()  // "hello, my name is Arto Hellas" gets printed
```

>[!tl/dr]
>To add methods to objects, write a function as the value of an object property

You can assign methods to an object even after th ecreation of the object:

```js
const arto = {
  name: 'Arto Hellas',
  age: 35,
  education: 'PhD',
  greet: function() {
    console.log('hello, my name is ' + this.name)
  },
}

arto.growOlder = function() {  this.age += 1}
console.log(arto.age)   // 35 is printed
arto.growOlder()
console.log(arto.age)   // 36 is printed
```

You can also assign a method to a variable, and execute the function by calling the variable with arguments passed in parentheses: 

```js
const arto = {
  name: 'Arto Hellas',
  age: 35,
  education: 'PhD',
  greet: function() {
    console.log('hello, my name is ' + this.name)
  },
  doAddition: function(a, b) {    console.log(a + b)  },}

arto.doAddition(1, 4)        // 5 is printed

const referenceToAddition = arto.doAddition
referenceToAddition(10, 15)   // 25 is printed
```

>[!info]
The above is called a method reference

>[!warning]
You can't do the same to (use a method reference on) the `greet()` method, as it references `this`. Since the method isn't being called on the object (the original `this`), JS takes `this` to mean the `global object`.

```js
arto.greet()       // "hello, my name is Arto Hellas" gets printed

const referenceToGreet = arto.greet
referenceToGreet() // prints "hello, my name is undefined"
```

In this course, we won't be using `this` as it causes problems.

The value of `this` in JavaScript is dependent on how the method is being called, and if the JavaScript engine is what is calling the method, as below, then `this` refers to the `global object`.

```js
const arto = {
  name: 'Arto Hellas',
  greet: function() {
    console.log('hello, my name is ' + this.name)
  },
}

setTimeout(arto.greet, 1000)
```
*`setTimeout is a JS built-in function (a global method)`*

You can preserve `this` using `.bind`. By using the below method, you create a new function where `this` is bound to `arto` irrespective of where it is being called:

```js
setTimeout(arto.greet.bind(arto), 1000)
```

You can solve some of the problems relating to `this` using arrow functions, but arrow functions should not be used as methods for objects because then `this` wouldn't work at all.

Classes
___
There is no class mechanism in JavaScript like in pure OOP languages, but there are features that allow you to 'simulate' opject-oriented classes.

This was introduced in ES6.

'Classes' in JavaScript are very reminiscent of Java classes objects. Their behaviour is also quite similar to Java objects. However at their core they are still objects based on JavaScript's prototypal inheritance.

The type of 'classes' in JS is sill `Object`, as JS only defines the data-types: Boolean, Null, Undefined, Number, String, Symbol, BigInt, and Object.

This syntax is used in old React and also in current Node.js, so it might come in handy later. Hooks do the job for us in modern React.

```js
class Person {
  constructor(name, age) {
    this.name = name
    this.age = age
  }
  greet() {
    console.log('hello, my name is ' + this.name)
  }
}

const adam = new Person('Adam Ondra', 29)
adam.greet()

const janja = new Person('Janja Garnbret', 23)
janja.greet()
```


