
A callback function is a function passed as an argument to another function to be executed later.

```js
function fetchData(callback) {
  setTimeout(() => {
    const data = 'Some data from the server';
    callback(data);
  }, 2000);
}

fetchData(data => {
  console.log(data); // Output: Some data from the server
});
```