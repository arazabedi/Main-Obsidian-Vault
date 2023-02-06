Start with this example:

```js
const Hello = (props) => {
  return (
    <div>
      <p>
        Hello {props.name}, you are {props.age} years old
      </p>
    </div>
  )
}

const App = () => {
  const name = 'Peter'
  const age = 10

  return (
    <div>
      <h1>Greetings</h1>
      <Hello name="Maya" age={26 + 10} />
      <Hello name={name} age={age} />
    </div>
  )
}
```

Component Helper Functions
___
Expand our Hello component so that it guesses the year of birth of the person being greeted:

```js
const Hello = (props) => {
  const bornYear = () => {
    const yearNow = new Date().getFullYear()
    return yearNow - props.age
  }

  return (
    <div>
      <p>
        Hello {props.name}, you are {props.age} years old
      </p>
      <p>So you were probably born in {bornYear()}</p>
    </div>
  )
}
```

The logic for the year of birth (`bornYear`) is separated into a function of its own that is called when the component is rendered.

The person's age can be directly accessed by the function through props.

Basically you make a variable and assign it a function. Then you call that variable in your return.

Destructuring
___
Since ES6, we can create new variables and assign a props' values to them (remember props are properties, which are objects).

```js
const Hello = (props) => {
  const name = props.name
  const age = props.age

  const bornYear = () => new Date().getFullYear() - age

  return (
    <div>
      <p>Hello {name}, you are {age} years old</p>
      <p>So you were probably born in {bornYear()}</p>
    </div>
  )
}
```

These two functions are the same (the top is ES6 syntax that has no curly braces becase there arrow function consists of only a single function):

```js
const bornYear = () => new Date().getFullYear() - age

const bornYear = () => {
  return new Date().getFullYear() - age
}
```

Destructuring makes variable assignment even easier since you can take a props (whatever has been fed as a prop, for exampe an object), and take the data from it, and assign those data points to variables with the same name as the data point.

For example, we can destructure the following prop object:

```js
props = {
  name: 'Arto Hellas',
  age: 35,
}
```

```js
const Hello = (props) => {
  const { name, age } = props
  const bornYear = () => new Date().getFullYear() - age

  return (
    <div>
      <p>Hello {name}, you are {age} years old</p>
      <p>So you were probably born in {bornYear()}</p>
    </div>
  )
}
```

The expression `const { name, age } = props` assigns 'Arto Hellas' to the variable `name`, and the number 35 to the variabel `age`.

You can alse take destructuring a step further:

```js
const Hello = ({ name, age }) => {
  const bornYear = () => new Date().getFullYear() - age

  return (
    <div>
      <p>
        Hello {name}, you are {age} years old
      </p>
      <p>So you were probably born in {bornYear()}</p>
    </div>
  )
}
```

This way, the props are directly destructured into the variables.

```js
//Instead of this:
const Hello = (props) => {
  const { name, age } = props
//We can do this:
const Hello = ({ name, age }) => {
```

Page re-rendering
___
Here we'll create a button-controlled counter like in the Svelte tutorials.

```js
//App.js
const App = (props) => {
  const {counter} = props
  return (
    <div>{counter}</div>
  )
}

export default App
```

```js
//index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

let counter = 1
ReactDOM.createRoot(document.getElementById('root')).render(
  <App counter={counter} />
)
```

Right not, if the counter variable has its value updated, this won't cause the counter component to re-render. We can get it to re-render by calling the `render` method a second time.

```js
let counter = 1

const refresh = () => {
  ReactDOM.createRoot(document.getElementById('root')).render(
    <App counter={counter} />
  )
}

refresh()
counter += 1
refresh()
counter += 1
refresh()
```
*notice that the render is wrapped in the `refresh()` function*

We can re-render and increment the counter also in this way:

```js
setInterval(() => {
  refresh()
  counter += 1
}, 1000)
```

This is not the recommended way to re-render components. We should instead use the `useState` hook.

Stateful Components
___
Let's change back out files:

```js
//index.js
import React from 'react'
import ReactDOM from 'react-dom/client'

import App from './App'

ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

```js
import { useState } from 'react'

const App = () => {
  const [ counter, setCounter ] = useState(0)

  setTimeout(
    () => setCounter(counter + 1),
    1000
  )

  return (
    <div>{counter}</div>
  )
}

export default App
```

Remember to always import hooks.

The following expression adds state to the component and initialises it with the value 0:

```js
const [ counter, setCounter ] = useState(0)
```

The inital value of `state` is zero, and the variable setCounter is assigned to a function that will be used to modify the state.

THe application called the setTimeout function and passes it two parameters: a function to increment the counter state and a timeout of one second:

```js
setTimeout(
  () => setCounter(counter + 1),
  1000
)
```

The function passed as the first parameter to the setTimeout function is invoked one second after calling the setTimeout function:

```js
() => setCounter(counter + 1)
```

When the state modifying function setCounter is called, React re-renders the component which means that the function body of the component function gets re-executed:

```js
() => {
  const [ counter, setCounter ] = useState(0)

  setTimeout(
    () => setCounter(counter + 1),
    1000
  )

  return (
    <div>{counter}</div>
  )
}
```

The second time the component function is executed it calles the useState function and returns the new value of the state: 1.

We can debug the application for render timing issues by logging the values of the component's variables to the console:

```js
const App = () => {
  const [ counter, setCounter ] = useState(0)

  setTimeout(
    () => setCounter(counter + 1),
    1000
  )

  console.log('rendering...', counter)

  return (
    <div>{counter}</div>
  )
}
```

Event Handlers
___
Standard click handler:

```js
const App = () => {
  const [ counter, setCounter ] = useState(0)

  const handleClick = () => {
    console.log('clicked')
  }

  return (
    <div>
      <div>{counter}</div>
      <button onClick={handleClick}>
        plus
      </button>
    </div>
  )
}
```

Increase counter by 1 on every click (and component gets re-rendered becuse it is  defined above using he useState hook):

```js
<button onClick={() => setCounter(counter + 1)}>
  plus
</button>
```

Hooks are **functions that let you “hook into” React state and lifecycle features from function components**. Hooks don't work inside classes — they let you use React without classes.

Event Handlers are Functions
___
Event handlers are *functions* that are set as the value for an attribute (e.g. the onClick attribute). We must include the bracket and arrow in order to make the code inside the curly braces an anonymous function, otherwise the app would break.

```js
<button onClick={() => setCounter(counter + 1)}> 
  plus
</button>
```

An event handler has to be a function or a function call. The above is a function.

If it was a function call it would look like this:

```js
<button onClick={setCounter(counter + 1)}>
```

This would cause an endless render loop because eachtime the counter updates, this would cause a re-render, which would cause the counter to update.

I am actually not sure why this loop occurs. Someone help!

The best way to do the counter:

```js
import { useState } from 'react'

const App = () => {
  const [ counter, setCounter ] = useState(0)

  const increaseByOne = () => setCounter(counter + 1)

  const setToZero = () => setCounter(0)

  return (
    <div>
      <div>{counter}</div>
      <button onClick={increaseByOne}>
        plus
      </button>
      <button onClick={setToZero}>
        zero
      </button>
    </div>
  )
}

export default App

```

*Separate out event handler functions and call those functions through their variable name in the onClick attribute (don't have functions as attribute values.*

Passing State to Child Components
___
Use small and reusable react components aross the applciation, and even projects.

https://github.com/brillout/awesome-react-components

Imagine actually coding your project yourself...

So make a smaller component:

```js
//Display.js
const Display = (props) => {
  return (
    <div>{props.counter}</div>
  );
}

export default Display;
```

Import and use it in the App.js file:

```js
import { useState } from 'react'
import Display from './Display'

const App = () => {
  const [ counter, setCounter ] = useState(0)

  const increaseByOne = () => setCounter(counter + 1)
  const setToZero = () => setCounter(0)

  return (
    <div>
      <Display counter={counter}/>
      <button onClick={increaseByOne}>
        plus
      </button>
      <button onClick={setToZero}>
        zero
      </button>
    </div>
  )
}

export default App
```

Make sure state is always in the parent components so that it can easily be passed down to its children through props. This is for some dumb reason called 'lifting state up'.

You can do this for the buttons too:

```js
//Button.js
const Button = (props) => {
  return (
    <button onClick={props.onClick}>
      {props.text}
    </button>
  );
}

export default Button;
```

```js
import { useState } from 'react'
import Display from './Display'
import Button from './Button'

const App = () => {
  const [ counter, setCounter ] = useState(0)

  const increaseByOne = () => setCounter(counter + 1)
  const decreaseByOne = () => setCounter(counter - 1)
  const setToZero = () => setCounter(0)

  return (
    <div>
      <Display counter={counter}/>
      <Button
        onClick={increaseByOne}
        text='plus'
      />
      <Button
        onClick={setToZero}
        text='zero'
      />
      <Button
        onClick={decreaseByOne}
        text='minus'
      />
    </div>
  )
}

export default App
```

Changes in State Cause Re-Rendering
___
Any change in state cause the entire compnent tree that is dependent on that state to be re-rendered, including subcomponents.

Refactoring the components
___
You can using *destructuring* to go from this:

```js
const Display = (props) => {
  return (
    <div>{props.counter}</div>
  )
}
```

To *this*:

```js
const Display = ({ counter }) => {
  return (
    <div>{counter}</div>
  )
}
```

If you have a component function with only a return statement, you can even make it a one liner:

```jsx
const Display = ({ counter }) => <div>{counter}</div>
```

We can also simplify the Button component from this:

```js
const Button = (props) => {
  return (
    <button onClick={props.onClick}>
      {props.text}
    </button>
  )
}
```

To this:

```js
const Button = ({ onClick, text }) => (
  <button onClick={onClick}>
    {text}
  </button>
)
```

>[!info]
>Basically, instead of writing props, simply add curly braces with the data points you need for the component inside (separated by commas)

One liner:

```js
const Button = ({ onClick, text }) => <button onClick={onClick}>{text}</button>
```

I would avoid the one liners as it's harder to read.