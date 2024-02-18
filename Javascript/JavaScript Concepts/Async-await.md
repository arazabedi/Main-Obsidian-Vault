The `async` and `await` keywords in JavaScript provide a simpler syntax for working with asynchronous functions.

```js
async function fetchData() {
  const response = await fetch('https://api.example.com/data');
  const data = await response.json();
  return data;
}

fetchData().then((data) => {
  console.log(data);
});
```

