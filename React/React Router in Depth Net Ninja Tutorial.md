React is a framework for creating single-page applications, and so it needs a way of 'routing' to different pages that doesn't involve sending html documents.

This is where the `react-router-dom` comes in. It stops a http request from reaching the server, and swaps out page content using JavaScript.

Each page is a React component.

All of this is handled on the client-side.

We will be making a simple react application that lists a bunch of jobs, which a page for each job and its details.

We've got a help page with nested routes, and we'll also include breadcrummbs.

We'll also learn about loaders, error components, forms, and actions.

## React Router Basics
___
First install react-router-dom:

```bash
npm install react-router-dom
```

Then import the 

```js
//app.js
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom'
```

BrowserRouter - Wraps the entire application and allows us to use the router within it
Routes, Route - Allow us to set-up routes
Link - Allows us to provide links to other pages

Now wrap the entire application with BrowserRouter:

```js
 import { BrowserRouter, Routes, Route, Link } from 'react-router-dom'

function App() {
  return (
		<BrowserRouter>

		</BrowserRouter>
  );
}

export default App
```

Now set-up the routes and specify which components should be rendered for each route:

![[Pasted image 20230210212725.png]]

*Note that no '/' is need for the path, React assumes this already.*

I've used an image above because the nesting made the formatting all wrong.

Go and make your components, then import them:

![[Pasted image 20230210213821.png]]

Now we can add a Link and a NavLink:

![[Pasted image 20230210220604.png]]

*Note that the index prop is equivalent to `path="about"`*

The difference between the Link and NavLink?

Ont the home page:
![[Pasted image 20230210220728.png]]

On the about page:

![[Pasted image 20230210220833.png]]

It's for styling when a particular page is being shown. For example you can highlight the current page link.

**BUT...**

This is the old way...

## Route Provider, createBrowserRouter, and Outlet
___
We're going to have to set up our router slightly differently.

