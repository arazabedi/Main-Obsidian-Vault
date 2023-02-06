Instead of this:

```js
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)
```

You can put state into a single *object* variable:

```js
  const [clicks, setClicks] = useState({
    left: 0, 
    right: 0
  })
```

Now we have to change  the event handler functions. Use *object spread* syntax:

```jsx
const handleLeftClick = () => {
  const newClicks = { 
    ...clicks, 
    left: clicks.left + 1 
  }
  setClicks(newClicks)
}

const handleRightClick = () => {
  const newClicks = { 
    ...clicks, 
    right: clicks.right + 1 
  }
  setClicks(newClicks)
}
```

The followvalueing creates a new object that has copies of all the properties of the `clicks` object. We can specify a particular propery as well if we want to change its 

```js
// exact copy
{...clicks}
// specifying a specific property value
{ ...clicks, right: clicks.right + 1 }
```

*Remember these are copies of the original `clicks` object*

Now the event handlers will look like this:

```jsx
const handleLeftClick = () =>
  setClicks({ ...clicks, left: clicks.left + 1 })

const handleRightClick = () =>
  setClicks({ ...clicks, right: clicks.right + 1 })
```

>[!warning]
>In React, we ***ALWAYS*** change state by setting the state to a new object. If properties from the previous stae object are not changed, then just copy those properties to the new object and set that as state. Directly muatting state can cause unexpected side-effects.

https://stackoverflow.com/questions/37755997/why-cant-i-directly-modify-a-components-state-really/40309023#40309023

Handling Arrays
___
Lets say we wanted the following (every time you click a button in adds to its respective counter, and notes down the button that you clicked):

![[Pasted image 20230205135016.png]]

Every click is stored in a separate piece of state called allClicks that is initialised as an empty array:

```js
const [allClicks, setAll] = useState([])
```

When the left button is clicked, we add the letter L to the allClicks array:

```js
const handleLeftClick = () => {
  setAll(allClicks.concat('L'))
  setLeft(left + 1)
}
```

The piece of state stored in allClicks is now set to be an array that cointains all of the items of the previous state array plus the letter L. Adding the new item to the array using concat - which returns a *new copy of the array* witht he item added to it.

>[!tip]
>Always use `concat` or another method that returns a new array (e.g. `map`, `filter` etc.) - ***NOT*** one that mutates an existing array

We've used the `join` method to put all the array elements onto a single string (separated by an empty space, as determined by the second argument of the method).

Conditional Rendering
___
You can chuck the history of the clicks into a separate History component:

```jsx
const History = (props) => {
  if (props.allClicks.length === 0) {
    return (
      <div>
        the app is used by pressing the buttons
      </div>
    )
  }
  return (
    <div>
      button press history: {props.allClicks.join(' ')}
    </div>
  )
}

export default History;
```

We've added an if statement to render the message `the app is used by pressing the buttons`  if no button has been clicked, and the press history if one has been clicked.

```jsx
import { useState } from 'react'

const History = (props) => {
  if (props.allClicks.length === 0) {
    return (
      <div>
        the app is used by pressing the buttons
      </div>
    )
  }
  return (
    <div>
      button press history: {props.allClicks.join(' ')}
    </div>
  )
}

const Button = ({ handleClick, text }) => (
  <button onClick={handleClick}>
    {text}
  </button>
)

const App = () => {
  const [left, setLeft] = useState(0)
  const [right, setRight] = useState(0)
  const [allClicks, setAll] = useState([])

  const handleLeftClick = () => {
    setAll(allClicks.concat('L'))
    setLeft(left + 1)
  }

  const handleRightClick = () => {
    setAll(allClicks.concat('R'))
    setRight(right + 1)
  }

  return (
    <div>
      {left}
      <button onClick={handleLeftClick}>left</button>
      <button onClick={handleRightClick}>right</button>
      {right}
      <History allClicks={allClicks} />
    </div>
  )
}

export default App
```

*Button component added*

Old React
___
useState is new, before this hook (and other hooks), you couldn't add *state* to functional components (modern React is quite functional). You used to have to define *state* as class components using JS class syntax.

Class syntax will be explored later in the cause as it pertains to maintaining backdated code.

Debugging React Applications
___
If a component isn't rendering: console.log its variables.

Separate items in a console.log by using a comma:

```js
console.log('props value is', props)
```

If you concatenate a string with an object using a +, you will log something like:

```js
// bad console.log
console.log('props value is ' + props)
//bad log
props value is [object Object]
```
*Not useful*

You can also pause the executing of your application code in the Chrome developer console's *debugger* by writing the command `debugger` anywhere in your code.

The execution will pauce at the point of the command.

In the *Console* tab you can inspect the current state of variables.

The debugger also allows you to execute code line byline in the *Sources tab*.

You can forgoe the use of the `debugger` command by adding breakpoints in the *Sources* tab inspect the values of the component's variables in the *Scope* section.

Also just use React dev tools.

Rules of Hooks
___
A few limitations and rules.

useState (and useEffect) can't be used inside of a *loop*, *conditional expression*, or other place that is not a function defining component. This is to ensure that the hooks are always called in the same order - or the app will behave erratically.

>[!warning]
>Hooks can ***ONLY*** be called from inside a component defining function, and:
>- ***Not*** in loops
>- ***Not*** in if statements or other conditionals

```jsx
const App = () => {
  // these are ok
  const [age, setAge] = useState(0)
  const [name, setName] = useState('Juha Tauriainen')

  if ( age > 10 ) {
    // this does not work!
    const [foobar, setFoobar] = useState(null)
  }

  for ( let i = 0; i < age; i++ ) {
    // also this is not good
    const [rightWay, setRightWay] = useState(false)
  }

  const notGood = () => {
    // and this is also illegal
    const [x, setX] = useState(-1000)
  }

  return (
    //...
  )
}
```

Event Handling Revisited
___
Basically just use arrow functions in event handler functions. Avoid putting the arrow function directly as the value of an onClick attribute.

A Function that Returns a Function
____
You can also define an event handler by using a function that returns a function.

It's probably unneccesary.

```js
const App = () => {
  const [value, setValue] = useState(10)

  const hello = () => {
    const handler = () => console.log('hello world')
    return handler
  }

  return (
    <div>
      {value}
      <button onClick={hello()}>button</button>
    </div>
  )
}
```

You call the hello function, which is itself returning an arrow function:

```js
<button onClick={hello()}>button</button>
```

What's the point?

If you want to pass an argument into a event handler, this would be the way to do it:

```js
const App = () => {
  const [value, setValue] = useState(10)

  const hello = (who) => {
    const handler = () => {
      console.log('hello', who)
    }
    return handler
  }

  return (
    <div>
      {value}
      <button onClick={hello('world')}>button</button>
      <button onClick={hello('react')}>button</button>
      <button onClick={hello('function')}>button</button>
    </div>
  )
}
```

>[!info]
>Use these 'function' functions to customise generic functionality of event handlers with parameters. In the above case, the `hello` function is a reusable function that produces customised events for greeting users.

You can condense:

```js
const hello = (who) => {
  return () => {
    console.log('hello', who)
  }
}
```

By ommiting curly braces (you can do this because there's only one return command):

```js
const hello = (who) =>
  () => {
    console.log('hello', who)
  }
```

To the even more compact:

```js
const hello = (who) => () => {
  console.log('hello', who)
}
```

You can use the same trick to define event handler that set the state of the component to a given value:

```jsx
const App = () => {
  const [value, setValue] = useState(10)
  
  const setToValue = (newValue) => () => {
    console.log('value now', newValue)  // print the new value to console
    setValue(newValue)
  }
  
  return (
    <div>
      {value}
      <button onClick={setToValue(1000)}>thousand</button>
      <button onClick={setToValue(0)}>reset</button>
      <button onClick={setToValue(value + 1)}>increment</button>
    </div>
  )
}
```

But you don't need functions that return functions for this:

```jsx
const App = () => {
  const [value, setValue] = useState(10)

  const setToValue = (newValue) => {
    console.log('value now', newValue)
    setValue(newValue)
  }

  return (
    <div>
      {value}
      <button onClick={() => setToValue(1000)}>
        thousand
      </button>
      <button onClick={() => setToValue(0)}>
        reset
      </button>
      <button onClick={() => setToValue(value + 1)}>
        increment
      </button>
    </div>
  )
}
```

Either is fine. The latter is less verbose.

Passing Event Handlers to Child Components
___
Put the button into its own component:

```jsx
const Button = (props) => (
  <button onClick={props.handleClick}>
    {props.text}
  </button>
)
```

```jsx
const App = (props) => {
  // ...
  return (
    <div>
      {value}
      <Button handleClick={setToValue(1000)} text="thousand" />
      <Button handleClick={setToValue(0)} text="reset" />
      <Button handleClick={setToValue(value + 1)} text="increment" />
    </div>
  )
}
```

>[!warning]
>Do not define components inside components!

