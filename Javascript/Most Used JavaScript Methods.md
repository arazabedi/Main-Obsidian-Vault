When working with arrays, numbers, objects, strings etc., you might want to check out this library: https://lodash.com/

And this:
https://github.com/smartprocure/futil-js

**Array Methods**
___
**.map**
```js
const newArray= Array.prototype.map(element => function)
```
*Apply a function to every element of the array and return in a new array*

**.filter**
```js
const newArray = Array.prototype.filter(element => condition)
```
*Return a new array only containing the elements for which the condition is true*

**.some()**
```js
const newArray = Array.prototype.some(element => condition)
```
*Return true if the condition is true for at least one element of the array*

**.every()**
```js
const newArray = Array.prototype.some(element => condition)
```
Return true id the condition is true for every element in the array

>[!info]
>The *condition* will look something like:
>`element > 5` or `element == false`

**.from()**
```js
const newArray = Array.from(object,function)
```
Return a new shallow-copied array from any iteratable or array-like object (optionally add a function to each element):

>[!example]
>```js
>console.log(Array.from('foo'));
>// ["f", "o", "o"]
>
console.log(Array.from([1, 2, 3], x => x + x));
// Expected output: Array [2, 4, 6]
>```

**.some()**

```js
Array.prototype.some(function)
```

Returns true if there exists an element for which the provided function returns true, false otherwise.

>[!example]
>```js
>const array = [1, 2, 3, 4, 5];
>
>// Checks whether an element is even
const even = (element) => element % 2 === 0;
>
console.log(array.some(even));
>// Expected output: true







