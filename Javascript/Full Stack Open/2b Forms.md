Here is some code with an added input section:

The `addNote` event handler uses the `preventDefault()` method to prevent the default action of submitting a form, which would, among other things, cause the page to reload.

The `event` parameter is the event that triggers the call the the event handler function.

```jsx
import { useState } from 'react'
import Note from "./components/Note"

const App = (props) => {
  const [notes, setNotes] = useState(props.notes)

  const addNote = (event) => {
    event.preventDefault()
    console.log('button clicked', event.target)
  }

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note =>
          <Note key={note.id} note={note} />
        )}
      </ul>
      <form onSubmit={addNote}>
        <input />
        <button type="submit">save</button>
      </form>
    </div>
  )
}

export default App
```

![[Pasted image 20230207140448.png]]

Controlled Component
___
How do we access data that's typed into the input element?

The first method we'll look at is by using *controlled components*.

Add a new piece of state called newNote for storing the user-submitted input, then set it as the input's value attribute.

This means that the component App now controlls the behaviour of the input element. As is, there will be an error (the input field won't be editable):

```jsx
import { useState } from 'react'
import Note from "./components/Note"

const App = (props) => {
  const [notes, setNotes] = useState(props.notes)
  const [newNote, setNewNote] = useState(
    'a new note...'
  )

  const addNote = (event) => {
    event.preventDefault()
    console.log('button clicked', event.target)
  }

  const handleNoteChange = (event) => {
    console.log(event.target.value)
    setNewNote(event.target.value)
  }

  return (
    <div>
      <h1>Notes</h1>
      <ul>
        {notes.map(note =>
          <Note key={note.id} note={note} />
        )}
      </ul>
      <form onSubmit={addNote}>
        <input
          value={newNote}
          onChange={handleNoteChange}
        />
        <button type="submit">save</button>
      </form>
    </div>
  )
}

export default App
```

The `onChange` attribute, when set to the `handleNoteChange` function, changes the value of the `newNote` variable on every change of the input bar's text.

You don't need a `preventDefault()` method here because input is not the same as submission (there is no default action).

We can now complete the `addNote` function for creating new notes.

Filtered Displayed Elements
___
```jsx
import { useState } from 'react'
import Note from "./components/Note"

const App = (props) => {
  const [notes, setNotes] = useState(props.notes)
  const [newNote, setNewNote] = useState(
    'a new note...'
  )
  const [showAll, setShowAll] = useState(true)

  const notesToShow = showAll
  ? notes
  : notes.filter(note => note.important === true)

  const addNote = (event) => {
    event.preventDefault()
    const noteObject = {
      content: newNote,
      important: Math.random() < 0.5,
      id: notes.length + 1,
    }

    setNotes(notes.concat(noteObject))
    setNewNote('')
  }

  const handleNoteChange = (event) => {
    console.log(event.target.value)
    setNewNote(event.target.value)
  }

  return (
    <div>
      <h1>Notes</h1>
      <div>
        <button onClick={() => setShowAll(!showAll)}>
          show {showAll ? 'important' : 'all' }
        </button>
      </div>
      <ul>
        {notesToShow.map(note =>
          <Note key={note.id} note={note} />
        )}
      </ul>
      <form onSubmit={addNote}>
        <input
          value={newNote}
          onChange={handleNoteChange}
        />
        <button type="submit">save</button>
      </form>
    </div>
  )
}

export default App
```

![[Pasted image 20230207144925.png]]

>[!tip]
>So basically...
>1. Your form element needs an onSubmit attribute with value of a function to add a name to your list
>2. Your input element needs a value (whatever is inputted), and an onChange attribute with a function to change the value on every change of the input field
>3. Your button needs a `type="submit"` for some reason.
>4. You can use `Math.random()` or a Date API as the key


