In JavaScript, the `map()` method is used to iterate over an array and perform an operation on each element. It returns a new array with the results of applying the provided function to each element of the original array.

Example:

```javascript
const numbers = [1, 2, 3, 4];
const doubled = numbers.map(num => num * 2);
console.log(doubled); // Output: [2, 4, 6, 8]
