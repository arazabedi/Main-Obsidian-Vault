The spread operator (`...`) in JavaScript is used to expand elements of an iterable (like arrays) into individual elements.

```js
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];

const combinedArray = [...arr1, ...arr2];
console.log(combinedArray); // Output: [1, 2, 3, 4, 5, 6]
```