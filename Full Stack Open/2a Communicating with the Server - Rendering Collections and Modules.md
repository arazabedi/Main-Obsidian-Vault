Tips:

1. ***Fucking use console.log***
2. Use VSCode Snippets (e.g. `clog`)

From here on out, we will be using the functional programming operators of the JavaScript array, such as `find`, `filter`,  and `map` - all the time.

3. Learn functional programming:
[[1. Higher Order Functions]]
[[2. Map]]
[[3. Reduce Basics]]
[[4. Reduce Advanced]]
[[5. Closures]]
[[6. Currying]]
[[7. Recursion]]
[[8.  Promises]]
[[9.  Functors]]
[[10.  More Functors]]
[[11.  Streams]]
[[12.  Monads]]

https://www.youtube.com/playlist?list=PL0zVEGEvSaeEd9hlmCXrk5yUyqUag-n84

Key Attribute
___
A “_key_” is a special string _attribute_ you need to include when creating lists of elements:

```js
const App = (props) => {
  const { notes } = props

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <li key={note.id}>            {note.content}
          </li>
        )}
      </ul>
    </div>
  )
}
```

The below would show an error:

```js
{notes.map(note => 
  <li>
	{note.content}
  </li>
```

Map
___
Map creates a new array from elements in another array which have a function executed on them.

The notation for map is a compact arrow function:

```js
note => note.id
```

Comes from:

```js
(note) => {
  return note.id
}
```

The function has a note object as a parameter and returns the value of its id field.

Anti-pattern: Array Indexes as Keys
___
You can also avoid the aboe error by using the array indexes as keys. Just add a second parameter to the map method:

```js
notes.map((note, i) => ...)
```

*i is the index at each position*

So here's another error-less way of creating lists:

```jsx
<ul>
  {notes.map((note, i) => 
    <li key={i}>
      {note.content}
    </li>
  )}
</ul>
```

>[!info]
>Essentially, always give list elements (html tags) a key attribute with a value corresponding to the index of the array element.

Refactoring Modules
___
Use destructuring to get `notes` from props directly:

```jsx
const App = ({ notes }) => {
  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <li key={note.id}>
            {note.content}
          </li>
        )}
      </ul>
    </div>
  )
}
```

We can separate out each note as a component:

```js
const Note = ({ note }) => {
  return (
    <li>{note.content}</li>
  )
}

const App = ({ notes }) => {
  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note => 
          <Note key={note.id} note={note} />
        )}
      </ul>
    </div>
  )
}
```

>[!warning]
>Note that the _key_ attribute must now be defined for the _Note_ components, and not for the _li_ tags like before.

Convention is to create a components folder and put it into src. I think apart from App.js



