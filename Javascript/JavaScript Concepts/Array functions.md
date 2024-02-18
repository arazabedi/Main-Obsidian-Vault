Array functions in JavaScript, such as `map()`, `filter()`, `reduce()`, `forEach()`, etc., provide convenient ways to work with arrays.

```js
const numbers = [1, 2, 3, 4];

const doubled = numbers.map(num => num * 2);
console.log(doubled); // Output: [2, 4, 6, 8]

const evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers); // Output: [2, 4]

const sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // Output: 10

```