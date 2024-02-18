
Lexical scoping refers to the way variables are resolved in nested functions based on their position in the code.

```js
function outerFunction() {
  const outerVariable = 'Outer';

  function innerFunction() {
    const innerVariable = 'Inner';
    console.log(outerVariable); // Accessible
  }

  innerFunction();
}

outerFunction();

```