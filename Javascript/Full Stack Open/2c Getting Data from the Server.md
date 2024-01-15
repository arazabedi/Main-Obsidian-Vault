We're going to be using a json file as the database.

Create a json file and populate it, like so:

```json
{
  "notes": [
    {
      "id": 1,
      "content": "HTML is easy",
      "important": true
    },
    {
      "id": 2,
      "content": "Browser can execute only JavaScript",
      "important": false
    },
    {
      "id": 3,
      "content": "GET and POST are the most important methods of HTTP protocol",
      "important": true
    }
  ]
}
```

Use the json-server library so we can fetch data from the json.

Global install (whole computer)
```bash
json-server --port 3001 --watch db.json
```

Directory install:
```bash
npx json-server --port 3001 --watch db.json
```

The `--watch` automatically checks for changes.

You can see the whole JSON in the *Resources* directory.

![[Pasted image 20230217151922.png]]

![[Pasted image 20230217152022.png]]

	Remember we've set the json server to localhost 3001

We will be using the axios library to communicate between the browser and server.

```bash
npm install axios
```

![[Pasted image 20230217152651.png]]

>[!info]
Node V18 has fetch built in.

Also install json-server as a development dependency *only used for developement:* 

```bash
npm install json-server --save-dev
```

Add the server line below to the *package.json* file:

```json
{
  // ... 
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject",
    "server": "json-server -p3001 --watch db.json"  },
}
```

This allows us to run the server using the following command:

```bash
npm run server
```

There is a difference between these two commands:
```js
npm install axios
npm install json-server --save-dev
```

1. Axios is installed as a runtime dependency of the application (we've done this because the programme itself needs this library in order to work)
2. Json-server is installed as a development dependency because the programme doesn't reuqire it (we're just using it to aid in the development)

## Axios and Promises
___
Add this to the `index.js` file:

```js
import axios from 'axios'

const promise = axios.get('http://localhost:3001/notes')
console.log(promise)

const promise2 = axios.get('http://localhost:3001/foobar')
console.log(promise2)
```

The Axios `get` method returns a *promise* object.

The promise object represents the eventual completion or failure of an asynchronous operation.

A promise can have one of three distinct states at any one point in time:

1. Pending - final value not yet available
2. Fulfilled - the operation has been succesfully completed and the final value is available. (This is also called *resolved.*)
3. Rejected - An error prevented the final value from being determined - the operation has failed

Right now we are getting this in the console log:

![[Pasted image 20230217153940.png]]

We have a fulfilled promise due to the succeful get request to `http://localhost:3001/notes`.

We also have a rejected promise - leading to a logged error (Uncaught), as the server has no `http://localhost:3001/foobar` resource location.

To access the result of the operation, we must register an event handler to the promise:

```js
const promise = axios.get('http://localhost:3001/notes')

promise.then(response => {
  console.log(response)
})
```

We now get the following log:

![[Pasted image 20230217154346.png]]

The browser has logged the value of the fulfilled promise as we have taken the promise object and applied the `.then()` method. We have passed this method as a first argument, a callback function, that logs the value of the promise object.

We don't need to store the object in a variable:

```js
axios.get('http://localhost:3001/notes').then(response => {
  const notes = response.data
  console.log(notes)
})
```

The callback function takes the data stored in the response, stores it in a variable, and prints the notes to the console.

This is a more readable way to write it:

```js
axios
  .get('http://localhost:3001/notes')
  .then(response => {
    const notes = response.data
    console.log(notes)
  })
```

The server returns plain text, however the axios library parses the datat into a JS array because the content-type header is *application/json; charset=utf-8*.

Now we can fetch the data.

## Effect-Hooks
___
We've already seen `useState`, now let's take a look as `useEffect`.

The Effect Hook lets you perform side effects on function components. 

Side effect examples are data fetching, setting up a subscription, manually changing the DOM etc.

Simplify the index.js to:

```js
ReactDOM.createRoot(document.getElementById('root')).render(<App />)
```

We do this as we aren't sending props to App.js.

Add the imports to App.js:
```js
import { useState, useEffect } from 'react'import axios from 'axios'import Note from './components/Note'
```

Empty out the useState since we no longer have props (set it to an empty array):

```js
const [notes, setNotes] = useState([])
```

Add the useEffect lines:

```js
  useEffect(() => {    console.log('effect')    axios      .get('http://localhost:3001/notes')      .then(response => {        console.log('promise fulfilled')        setNotes(response.data)      })  }, [])  console.log('render', notes.length, 'notes')
```

Everything else stays the same (we are only adding json data to the existing component):

![[Pasted image 20230217160520.png]]
This is our log:
![[Pasted image 20230217160557.png]]

1. The body the function defining the component is rendered for the first time - `render 0 notes` is logged
2. Immediately after rendering, the following effect (React-speak for function) is executed:
```js
() => {
  console.log('effect')
  axios
    .get('http://localhost:3001/notes')
    .then(response => {
      console.log('promise fulfilled')
      setNotes(response.data)
    })
}
```

First, `effect` is logged, then the `axios.get` command fetches the data from the server and registers the following function as an *event handler* for the operation:

```js
response => {
  console.log('promise fulfilled')
  setNotes(response.data)
})
```

When the data arrives from the server, the browser calls the event handler function which logs `promise fulfilled` and stores the notes using the function `setNotes`. 

We can re-write as:

```js
const hook = () => {
  console.log('effect')
  axios
    .get('http://localhost:3001/notes')
    .then(response => {
      console.log('promise fulfilled')
      setNotes(response.data)
    })
}

useEffect(hook, [])
```

The function useEffect takes two parameters. The function (effect) itself, and the second to specify how often the effect is run (an empty array is used to indicate that the effect only runs along the first render of the component).

We could have also written:

```js
useEffect(() => {
  console.log('effect')

  const eventHandler = response => {
    console.log('promise fulfilled')
    setNotes(response.data)
  }

  const promise = axios.get('http://localhost:3001/notes')
  promise.then(eventHandler)
}, [])
```

This does not save any data to the server. We will see that in part 2d.

The architecture of the application:

![[Pasted image 20230217161406.png]]


