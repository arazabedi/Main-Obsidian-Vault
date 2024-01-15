**REST**
___
*Json-server* is not strictly speaking RESTful but neither are most REST APIs.

REST is a convention for the use of routes (URLs) and HTTP request types.

In RESTful APIs:

1. Individual data objects (e.g. notes) are resources.
2. A resourse has a unique url:

Using json-server:
- */notes* - returns a resource collection containing all notes.
- /notes/3 - returns an individual notes with the id 3

3. Resources are fetched with `GET` requests.
4. Creating a new resource for storing a note is done using a `POST` request to the *notes* url (the data is sent in the *body* of the request).

Json-server requires all data to be sent in the JSON format:
1. Data-Type: string
2. Content-Type: application/json


**Sending Data to the Server**
___
Add the axios post request to your addNote function.

```jsx
  const addNote = (event) => {
    event.preventDefault();
    const noteObject = {
      content: newNote,
      important: Math.random() < 0.5,
    };

    axios.post("http://localhost:3001/notes", noteObject).then((response) => {
			setNotes(notes.concat(noteObject));
			setNewNote("");
    });
  };
```

*Note that there is no id in the noteObject, as the json-server will determine this itself*

**Updating JSON Data**
___
1. Define the URL of each note resource based on its id (we will pass in the id as an argument).
2. Find the note we want to modify and assign it to the note variable
3. Create a new object, exactly the same as the old note (`...note`) and set its important value to the opposite of  what it was before.

```jsx
// ...
const toggleImportanceOf = (id) => {
	const url = `http://localhost:3001/notes/${id}`
	const note = notes.find(n => n.id === id)
	const changedNote = { ...note, important: !note.important }

	axios.put(url, changedNote).then(response => {
		setNotes(notes.map(n => n.id !== id ? n : response.data))
	})
}
//...
```

Note that this is using object spread syntax.

The following creates a new object with copies of all the properties from the `note` object:

```js
{...note}
```

We can then add properties inside the curly braces after the spread object.

```js
const changedNote = { ...note, important: !note.important }
```

This code creates a new object called `changedNote` based on an existing object called `note`. The `...` operator is called the spread syntax, and it's used to create a new object with all the properties of an existing object.

The new object `changedNote` will have all the properties of `note`, but the `important` property will have its value inverted using the `!` (not) operator. So if `note.important` is `true`, then `!note.important` will be `false`, and vice versa.

This code is a convenient way to create a new object with most of the properties of an existing object, while changing or adding one or more properties. In this case, the new object is identical to the original object, except that the value of the `important` property has been inverted.

Okay so that's the variables dealt with. Let's tale a look at the axios code next.

1. We send the changedNote to the url.
2. We change notes using setNotes - to a new array with all the items from the previous notes array except the old note which is replace by the updated version of it returned by the server.

```js
axios.put(url, changedNote).then(response => {
  setNotes(notes.map(note => note.id !== id ? note : response.data))
})
```

This third part is accomplished with the map method:

```js
notes.map((note) => note.id !== id ? note : response.data)
```

This creates a new array with a function being applied to each element of the existing array. If the element's id is not the same as the id given as the argument for the toggleImportanceOf function, then it simply returns itself, otherwise it returns the response data, which is the new object.

>[!tip]
>If you want to change only some part of an array, use the map function, and filter for the ones you want to change.

Next.........
Send the whoooole function as a prop to Note:
```jsx
<ul>
{notesToShow.map((note) => (
  <Note
	key={note.id}
	note={note}
	toggleImportance={() => toggleImportanceOf(note.id)}
  />
))}
</ul>
```

**Extracting Communication with the Backend into a Separate Module**
___
The App component is now bloated.

A component should be responsible for one thing (SRP https://en.wikipedia.org/wiki/Single-responsibility_principle).

Create a src/services directory.

Make some functions:

```js
import axios from 'axios'
const baseUrl = 'http://localhost:3001/notes'

const getAll = () => {
  return axios.get(baseUrl)
}

const create = newObject => {
  return axios.post(baseUrl, newObject)
}

const update = (id, newObject) => {
  return axios.put(`${baseUrl}/${id}`, newObject)
}

export default { 
  getAll: getAll, 
  create: create, 
  update: update 
}
```

Import them in App.js:

```js
import noteService from './services/notes'
const App = () => {
...
```

*noteService is an arbitrarily naming for the object exported at the end of notes.js.

Replace tha axios code in the App.js file with the functions imported from notes.js:

```js
const App = () => {
  // ...

  useEffect(() => {
    noteService      .getAll()      .then(response => {        setNotes(response.data)      })  }, [])

  const toggleImportanceOf = id => {
    const note = notes.find(n => n.id === id)
    const changedNote = { ...note, important: !note.important }

    noteService      .update(id, changedNote)      .then(response => {        setNotes(notes.map(note => note.id !== id ? note : response.data))      })  }

  const addNote = (event) => {
    event.preventDefault()
    const noteObject = {
      content: newNote,
      important: Math.random() > 0.5
    }

    noteService      .create(noteObject)      .then(response => {        setNotes(notes.concat(response.data))        setNewNote('')      })  }

  // ...
}

export default App
```

At this point it's better to understand than copy/paste.

`noteService` is an object. It has methods private to itself (this is getting into OOP territory).

The `noteService` objec is named by the developer. It can be called whatever you want. You are essentially importing any methods within another file. The way you do this is by importing the object, and then calling those methods on the object. 

We could also export the functions like this:

```js
export const getAll = () => {
  return axios.get(baseUrl);
};

export const create = (newObject) => {
  return axios.post(baseUrl, newObject);
};

export const update = (id, newObject) => {
  return axios.put(`${baseUrl}/${id}`, newObject);
};

```

And then import like this:

```js
import { getAll, create, update } from './services/notes'
```

Call those methods then use the responses to update the notes state variable with setNotes.

Don't forget to reset the newNote to an empty string.

For some reason we're going to re-write the module code:

```js
import axios from 'axios'
const baseUrl = 'http://localhost:3001/notes'

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

const create = newObject => {
  const request = axios.post(baseUrl, newObject)
  return request.then(response => response.data)
}

const update = (id, newObject) => {
  const request = axios.put(`${baseUrl}/${id}`, newObject)
  return request.then(response => response.data)
}

export default {
  getAll: getAll,
  create: create,
  update: update
}
```

This no longer returns the promise retured by axios directly. Instead, the promise is assigned to the request variable. We then use the `.then()` method to return the response data.

We can now modify App.js to work with this new module

```js
const App = () => {
  // ...

  useEffect(() => {
    noteService
      .getAll()
      .then(initialNotes => {
        setNotes(initialNotes)
      })
  }, [])

  const toggleImportanceOf = id => {
    const note = notes.find(n => n.id === id)
    const changedNote = { ...note, important: !note.important }

    noteService
      .update(id, changedNote)
      .then(returnedNote => {
        setNotes(notes.map(note => note.id !== id ? note : returnedNote))
      })
  }

  const addNote = (event) => {
    event.preventDefault()
    const noteObject = {
      content: newNote,
      important: Math.random() > 0.5
    }

    noteService
      .create(noteObject)
      .then(returnedNote => {
        setNotes(notes.concat(returnedNote))
        setNewNote('')
      })
  }

  // ...
}
```

Learn more about promises and async/await:
https://javascript.info/promise-chaining
https://github.com/getify/You-Dont-Know-JS/blob/1st-ed/async%20%26%20performance/ch3.md

It's kinda complicated but modern JS development uses it a lot so go and read you lazy shit.

**Cleaner Syntax for Defining Object Literals**
___
Above, we defined the default export of our notes.js module as an object:

```js
export default { 
  getAll: getAll, 
  create: create, 
  update: update 
}
```

*Remember is it's in curly braces, then it's a JavaScript object, which means you can do a bunch of object related stuff (i.e. methods) on it*

The labels to the left of the colon in the object are the keys of the object, wheras the ones to the right of it are *variables* defined inside the object.

In another file, when you call on the object's 'method', you're actually accessing the variable through its key.

You can write this using more compact syntax, since the keys and variables are of the same name:

```js
{ 
  getAll, 
  create, 
  update 
}
```

The module export can therefore look like this:

```js
export default { getAll, create, update }
```

Full code:
```js
import axios from 'axios'
const baseUrl = 'http://localhost:3001/notes'

const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

const create = newObject => {
  const request = axios.post(baseUrl, newObject)
  return request.then(response => response.data)
}

const update = (id, newObject) => {
  const request = axios.put(`${baseUrl}/${id}`, newObject)
  return request.then(response => response.data)
}

export default { getAll, create, update }
```

By using this shorter notation, we are using a new feature introduced in ES6. This is called the ***object initializer***. An object initializer is a comma-delimited list of zero or more pairs of prperty names and associated values of an object, enclosed in curly braces ( { } ). 

Essentially, we can create an object without using the old syntax:

```js
new Object()
//or
Object.create()
```

Object initializers, also known as object literals, are just a way to create object simply by surrounding the key-value comma-delimitted pairs by curly braces. I didn't realise this was so groundbreaking. Isn't this how you do it in Ruby?

```js
const person = { name: "John", age: 30, gender: "male" };
```

You can also use a variable as a key or value by putting it in square brackets:

```js
const propName = "age";
const person = { name: "John", [propName]: 30 };
```

You can also omit the property value id the property name is the same as a variable name that contains the value:

```js
const name = "John";
const age = 30;
const person = { name, age };
```

**Promises and Errors**
___
We change our module in the following way:

```js
//notes.js
const getAll = () => {
  const request = axios.get(baseUrl)
  const nonExisting = {
    id: 10000,
    content: 'This note is not saved to server',
    important: true,
  }
  return request.then(response => response.data.concat(nonExisting))
}
```

Since the getAll function is called bu useEffect in the App.js file, the site will display the nonExisting object's message, but the message won't actually exist in the json.

Therefore when you try to change the importance of the message using the toggle button, it won't work. The toggleImportanceOf function is as below: 

```js
  const toggleImportanceOf = (id) => {
    const note = notes.find((n) => n.id === id);
    const changedNote = { ...note, important: !note.important };

    noteService
      .update(id, changedNote)
      .then(returnedNote => {
        setNotes(notes.map(note => note.id !== id ? note : returnedNote))
      })
  };
```

It looks for a note in the app's state with the id of the button you clicked. It finds the object here. It flips the important property. Then, it does this:

```js
const update = (id, newObject) => {
  const request = axios.put(`${baseUrl}/${id}`, newObject)
  return request.then(response => response.data)
}
```

The http put request will fail, giving us a 404 error.

The promise is therefore rejected. This rejection is not currently being handled in any way.

We can handle it by providing the `then()` method with a second callback function (which executes in case the promise is rejected).

It's more common to use the .catch() method though:

```js
axios
  .get('http://example.com/probably_will_fail')
  .then(response => {
    console.log('success!')
  })
  .catch(error => {
    console.log('fail')
  })
```

Add a catch method to the App.js code:

```js
const toggleImportanceOf = id => {
  const note = notes.find(n => n.id === id)
  const changedNote = { ...note, important: !note.important }

  noteService
    .update(id, changedNote).then(returnedNote => {
      setNotes(notes.map(note => note.id !== id ? note : returnedNote))
    })
    .catch(error => {
      alert(
        `the note '${note.content}' was already deleted from server`
      )
      setNotes(notes.filter(n => n.id !== id))
    })
}
```

We also remove the note from the application's state using the filter method.

**The Full-Stack Developer's Oath**
___
![[Pasted image 20230221004836.png]]

