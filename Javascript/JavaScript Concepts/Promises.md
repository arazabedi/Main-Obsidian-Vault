Promises in JavaScript represent the eventual completion (or failure) of an asynchronous operation and its resulting value.

```js
const myPromise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('Promise resolved');
  }, 2000);
});

myPromise.then(result => {
  console.log(result); // Output: Promise resolved
});
```