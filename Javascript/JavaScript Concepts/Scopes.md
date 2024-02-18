
Scopes define the visibility and accessibility of variables within your code. In JavaScript, there are two main types of scope: global scope and local scope.

```js
let globalVariable = 'I am global';

function myFunction() {
  let localVariable = 'I am local';
  console.log(globalVariable); // Accessible
  console.log(localVariable); // Accessible
}

console.log(globalVariable); // Accessible
console.log(localVariable); // Not accessible

```