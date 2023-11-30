##### Higher-Order Functions
___
In JS, all functions are values.

This means that a function can be passed around.

For example, a function can be assigned to a variable or passed into another function (this is a higher-order function).

Higher-order functions are good for composition (composing lots of small functions into a big one).

Filter is a useful higher-order function.

You pass a function into the filter function which is used to filter out the elements of an array that don't hold true for the argument function.

This is a much simpler process than using a for loop.

Functions that you send into other functions are called callback functions.

Composability is the ability of separate functions to fit together to perform useful actions.

##### Map
___
Map does not throw objects away, but instead transforms them.

The callback function is applied to each element of the array.

##### Reduce
___
