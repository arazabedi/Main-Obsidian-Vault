Asynchronous programming in JavaScript allows code to run without blocking other operations, enabling non-blocking behaviour.

```js
console.log('Start');
setTimeout(() => {
  console.log('Middle');
}, 1000);
console.log('End');

// Output:
// Start
// End
// Middle (after 1 second)
```
