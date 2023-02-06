Creating a React App
___
```bash
npx create-react-app <name> // create a react application 
npm start // run the application and open in browser
```

>[!info]
>NPM is a package manager. It's how you _install_ and manage things like npx.
>
>NPX is a utility to _run_ other utilities, installing their prerequisites first if needed.

React Components
___
A file with a name such as `App.js` is a React component. The first letter must be capitalised. The name can be anything.

The following line in `index.js` renders the content of `App.js` into an element with the id 'root'. The HTML document where this element is defined is index.html, corresponding to index.js.

```js
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

Therefore:

1. Lower-case js files are for importing components for use in html files.
2. Upper-case js files are components

In React, all content to be viewed in the browser in defined as Reacr components.

A component is defined as a JavaScript function (you must export too):

```js
const App = () => (
  <div>
    <p>Hello world</p>
  </div>
)

export default App
```

Technically it is better convention to write using the following syntax, as you may need to add JS. For example:

```js
const App = () => {
	console.log("Hello world!")
	return (
		<div>
			<p>Hello world</p>
		</div>
	)
}

export default App
```

The JS can be dynamic, referring to the time, or adding sums together:

```js
const App = () => {
  const now = new Date()
  const a = 10
  const b = 20
  console.log(now, a+b)

  return (
    <div>
      <p>Hello world, it is {now.toString()}</p>
      <p>
        {a} plus {b} is {a + b}
      </p>
    </div>
  )
}

export default App
```

JSX
___
Even though it looks like React components are built using HTML, it is actually written in JSX (which looks like HTML) and compiled by Babel into JavaScript.

`create-react-app` projects are configured to compile automatically.

After compilation we ge thte following JS:

```js
const App = () => {
  const now = new Date()
  const a = 10
  const b = 20
  return React.createElement(
    'div',
    null,
    React.createElement(
      'p', null, 'Hello world, it is ', now.toString()
    ),
    React.createElement(
      'p', null, a, ' plus ', b, ' is ', a + b
    )
  )
}

export default App
```

In JSX, every tag needs to be closed, including ones with don't need to be closed in HTML, such as the break line tag below:

```html
<br> // html

<br /> jsx
```

You can define multiple components within one js file:

```js
const Hello = () => {  
	return (    
		<div>      
			<p>Hello world</p>    
		</div>  
	)
}

const App = () => {
  return (
    <div>
      <h1>Greetings</h1>
      <Hello />    </div>
  )
}
```

Props
___
You can pass data to components using props.

```jsx
const Hello = (props) => {
  console.log(props)
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
      <Hello name='Maya' age={26 + 10} />
      <Hello name={name} age={age} />
    </div>
  )
}
```

*Then browser will display two `Hello` components. *

Having `props` as the parameter of a component allows you to insert data into that component in the code of the return section of another component.

You do this by treating the data points the same way as html attributes.

Remember that in order for this to work, the return section of the component you want to insert data into must have those data points already noted:

```jsx
const Hello = (props) => {
...
	return (
		Hello {props.name}, you are {props.age}
...)
```
*Note that the parameter is just props, so props is always an object*

Insert:

```jsx
const App = () => {
...
	return (
		<Hello name='Maya' age={26 + 10} />
...)
```

>[!warning]
>All component names must have capitalised first letters.

>[!warning]
>All components must have a root element (usually a div, or empty element), or have the return in an array as opposed to parentheses, as below.

Fragment (empty element so no extra div is rendered):

```jsx
const App = () => {
  const name = 'Peter'
  const age = 10

  return (
    <>
      <h1>Greetings</h1>
      <Hello name='Maya' age={26 + 10} />
      <Hello name={name} age={age} />
      <Footer />
    </>
  )
}

```

Array:

```jsx
const App = () => {
  return [
    <h1>Greetings</h1>,
    <Hello name='Maya' age={26 + 10} />,
    <Footer />
  ]
}
```

>[!tip]
>Use fragments (<>)

React can only render primitive values such as numbers or strings. Arrays can be rendered if the array only contains primitive values.