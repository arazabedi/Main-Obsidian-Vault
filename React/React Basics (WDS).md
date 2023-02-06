To create a react app you need to be in a folder named with dashes instead of spaces (e.g. learn-react-in-x).

Create React App
___
```bash
npx create-react-app <name>
```

Start Development Server (for development)
___
```bash
npm run start
```

Build Application and Minify Files (for production)
```bash
npm run build
```

The application starts with index.js.

Render Component
___
```js
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

JSX
___
```jsx
//App.js
import React, { useState } from 'react';
import TodoList from './TodoList'

function App() {
	const [todos, setTodos] = useState({ id: 1, name: 'Todo 1', complete: false }])
	return (
		<>
			<TodoList todos={todos}/>
			<input type="text" />
			<button> Clear Completed Todos</button>
			<div>0 left to do</div>
		<>
	)
} 

export default App;

// snippet - rfc creates a function component with name of file name
```

A javascript function can only return one thing, so you must wrap multiple lines of JSX inside a fragment (an empty element). This tutorial is 3 years old so I think modern JSX requires wrapping with a div. 

You need the `useState()` hook to access state.  `useState()` returns an array.

In the above example, destructuring has been used https://www.youtube.com/watch?v=NIq3qLaHCIs. The first variable in the array (todos) is every todo in the todos state, and the second variable (setTodos) is the function we call to update those todos.

Props
___
In the above example, we have said that we have a *prop* `todos` on our Todolist, and we want to pass the `todos` variable from the useState line to the *prop* todos.

![A Simple Guide to Component Props in React](https://dmitripavlutin.com/react-props/cover.png)

Building a todo list
___
```jsx
//TodoList.js
import React from 'react'
import Todo from './Todo'

export default function TodoList({ todo }) {
return (
	todos.map(todo => {
		return key={todo} <Todo todo={todo} />
		})
	)
}
```

```jsx
import React from 'react'

export default function Todo({ todo }) {
	return (
		<div>
			{todo}
		<div>
	)
}
```

Keys
___
Use a `key` with a unique name to only re-render the components that actually change within the array.

Up to 13 mins.