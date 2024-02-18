Destructuring assignment is a syntax in JavaScript that allows you to extract values from arrays or objects and assign them to variables.

```js
const person = { name: 'John', age: 30 };
const { name, age } = person;

console.log(name, age); // Output: John 30

let a, b, rest;
[a, b] = [10, 20];
console.log(a);
// Expected output: 10
console.log(b);
// Expected output: 20

[a, b, ...rest] = [10, 20, 30, 40, 50];
console.log(rest);
// Expected output: Array [30, 40, 50]
```