##### Contents: #####
1. [[3a Node.js & Express#Simple Web Server|Simple Web Server]]
2. [[3a Node.js & Express#Express|Express]]
3. [[3a Node.js & Express#Web and Express|Web and Express]]
4. [[3a Node.js & Express#nodemon|nodemon]]
5. [[3a Node.js & Express#REST|REST]]
6. [[3a Node.js & Express#Fetching a Single Resource|Fetching a Single Resource]]
7. [[3a Node.js & Express#Deleting Resources|Deleting Resources]]
8. [[3a Node.js & Express#Postman|Postman]]
9. [[3a Node.js & Express#The VSCode REST Client|Visual Studio Code REST Plugin]]
10. [[3a Node.js & Express#The WebStorm HTTP CLient|Webstorm HTTP Client]]
11. [[3a Node.js & Express#Receiving Data|Receiving Data]]
12. [[3a Node.js & Express#HTTP Request Types|HTTP Request Types]]
13. [[3a Node.js & Express#Middleware|Middleware]]

##### Initialising our application #####
___
In part 3 we will shift our focus towards the backend. This means implementing functionality on the server side of the stack.

We will implement a simple REST API in Node.js by using the Express library, and the application's data will be stored in a MongoDB database.

At the end we will deploy our applicaiton to the internet.

First of all create an empty directory and use:

```bash
npm init
```

This will go through the process of creating a package.json for you.

Add the following script so that on the use of `npm run start`, the terminal runs Node.js for the index.js file.

![[Pasted image 20230221134806.png]]

You can now use either:

```bash
node index.js
//or
npm run start
```

*`npm run start` only works because we defined in the package.json file*

##### Simple Web Server #####
___
Create a web server using the following code. Node.js has a built in module for HTTP requests, you just have to require it:

```js
const http = require('http')

const app = http.createServer((request, response) => {
  response.writeHead(200, { 'Content-Type': 'text/plain' })
  response.end('Hello World')
})

const PORT = 3001
app.listen(PORT)
console.log(`Server running on port ${PORT}`)
```

An explanation of the above code is as follows.

1. We import Node's built-in web server module

```js
const http = require('http')
```

>[!info]
>This is slightly different from the way import are done for browser-side code, which would look like this: 
>```js
>import http from 'http'
>```
>The browser syntax is using ES6 modules, whereas Node.js uses CommonJS modules. Node also has imperfect support for ES6 modules - avoid them for now while they are not up to production standard.

2. We use the http module's createServer method. This method has an event handler fuunction as an argument, which is called every time an HTTP request is made to the server's address.

```js
const app = http.createServer((request, response) => {
  response.writeHead(200, { 'Content-Type': 'text/plain' })
  response.end('Hello World')
})
```

>[!info]
>In this case the request response contains the stats code 200 and the content-type is text/plain. The content returned to the site is *Hello World*

3. The last chunk of code applies the `listen()` method to the http server (the variable app), so that is listens to requests on port 3001, and logs the port number in the console:

```js
const PORT = 3001
app.listen(PORT)
console.log(`Server running on port ${PORT}`)
```

This course uses the backend server mainly to offer raw JSON data to the frontend.

Therefore, lets add some JSON formatted notes to the index.js file:

```js
const http = require('http')

let notes = [  {    id: 1,    content: "HTML is easy",    important: true  },  {    id: 2,    content: "Browser can execute only JavaScript",    important: false  },  {    id: 3,    content: "GET and POST are the most important methods of HTTP protocol",    important: true  }]const app = http.createServer((request, response) => {  response.writeHead(200, { 'Content-Type': 'application/json' })  response.end(JSON.stringify(notes))})
const PORT = 3001
app.listen(PORT)
console.log(`Server running on port ${PORT}`)
```

This will now display the JSON in string format in your browser.

>[!info]
>Remember the createServer method takes a callback function with two parameters, request and response,  which you can in your callback. For example you can log the request to the console with `request.log`, but mainly you would be using `writeHead(status code, object with headers)` and `.end(content to display on screen)`

##### Express #####
___
Node's built in http web server is cumbersome, so we will be using Express.js instead.

```bash
npm install express
```

This will be added to the depencies object of your `package.json`.

```json
//package.json
{
  // ...
  "dependencies": {
    "express": "^4.18.2"
  }
}
```

The source code for the dependency is installed in the *node_modules* directory at the root of your project. VScode hides this by default.

Many dependencies, including Express, have dependencies themselves, and those may have their own etc.

Our express version has a cart (^) prior to its number. This is because it's using a versioning model called (*semantic versioning*).

![[Pasted image 20230221151652.png]]

![[Pasted image 20230221151715.png]]

The caret means that when the dependecies of a project are updated, the version of express that is installed will be at least 4.18.2.

Use on our own computer:

```bash
npm update
```

On other computers:

```bash
npm install
```

##### Web and Express #####
___
Here's the updated *index.js* for use with express:

```js
const express = require('express')
const app = express()

let notes = [
	{
    id: 1,
    content: "HTML is easy",
    important: true
  },
  {
    id: 2,
    content: "Browser can execute only JavaScript",
    important: false
  },
  {
    id: 3,
    content: "GET and POST are the most important methods of HTTP protocol",
    important: true
  }
]

app.get('/', (request, response) => {
  response.send('<h1>Hello World!</h1>')
})

app.get('/api/notes', (request, response) => {
  response.json(notes)
})

const PORT = 3001
app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

1. Import express (which is a function):

```js
const express = require('express')
const app = express()
```

2. Define routes (`.get` take a url path, and a callback for what to do with the response and request):

```js
app.get('/', (request, response) => {
  response.send('<h1>Hello World!</h1>')
})
```

>[!info]
>Express.js automatically sets the value of the content-type header to text/html as the parameter for `.send()`is a string!

```js
app.get('/api/notes', (request, response) => {
  response.json(notes)
})
```

>[!info]
>Content-type set automatically to *application/json*.
> Express also does not require `JSON.stringify`, and it transforms the JSON automatically.

##### nodemon #####
___
You can use nodemon to watch the files in the directory and automatically restart node if anything changes:

```bash
npm install --save-dev nodemon
```

This will include nodemon only under development dependencies so that we don't include unnecessary libraries in our production application.

Add a dev script so you can run nodemon easily.

```json
{
  // ..
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  // ..
}
```

>[!warning]
>Nodemon restarts the server but doesn't hot reload the browser.

With the start and test script we can just write:

```bash
npm start
//and
npm test
```

But for the dev script we have to add the `run` command.

##### REST #####
___
We're going to make our application into a RESTful HTTP API.

Singular things such as an individual note are called *resources* in REST. 

Every resource has an associated URL. This is the resource's unique address.

The convention we will use is by combing the type of a resource and its unique identifier.

For example if our resource is a note, and it has an ID of 10, then it has the unique address:

www.example.com/api/notes/10

The URL for the entire collection of notes is:

www.example.com/api/notes

See the below cheatsheet:

![[Pasted image 20230221170517.png]]

This is the architecture for a straightforward CRUD API, and some may refer to it as an aexample of resource-oriented architecture rather than REST. We won't bother arguing over semantics. However this falls under the second level of RESTful maturity in the Richardson Maturity Model.

##### Fetching a Single Resource #####
___
Lets's start by creating a route for fetching a single resource:

```js
app.get('/api/notes/:id', (request, response) => {
  const id = request.params.id
  const note = notes.find(note => note.id === id)
  response.json(note)
})
```

This block will handle any get request coming in to the the /api/notes/anything url path.

This won't actually work as id is a string and notes.id is. a number, so fix this:

```js
app.get("/api/notes/:id", (request, response) => {
  const id = Number(request.params.id);
  const note = notes.find((note) => {
    return note.id === id;
  });
  console.log(note);
  response.json(note);
});
```

But this returns a 200 status code for unfound resources (ids that don't exist). Change this to 404.

```js
app.get("/api/notes/:id", (request, response) => {
  const id = Number(request.params.id);
  const note = notes.find((note) => note.id === id);

  if (note) {
    response.json(note);
  } else {
    response.status(404).send("No such resource");
  }
});
```

##### Deleting Resources #####
___
Next let's implement a route for deleting resources. 

Deletion happens when making a HTTP DELETE request to the URL of the resource.

```js
app.delete('/api/notes/:id', (request, response) => {
  const id = Number(request.params.id)
  notes = notes.filter(note => note.id !== id)

  response.status(204).end()
})
```

A succesful deletion means that the note exists and is removed, and we respond to the request with the 204 status code (no content), and return no data with the reponse.

If the resource doesn't exist then we can respond with either 204 or 404. 204 is used in this course.

##### Postman #####
___
We use Postman to test the delete operation.

HTTP GET requests are easy to make from the browser, and we could write some JavaScript to test deletion, but writing test code is not always the best solution in every situation.

Curl can be used also, but Postman has a nice graphical interface.

Just type in the URL and choose the DELETE option.

It should have worked.

In our current application we have no database and the data is coded into index.js so it will return on a server restart.

##### The VSCode REST Client #####
___
I've downloaded the VSCode REST plugin, which allows you to make HTTP requests from within `.rest` files.

Just type in something like:

```
GET http://localhost:3001/api/notes/
```

And a little 'send request' button will appear above that line.

##### The WebStorm HTTP Client #####
___
WebjStorm has a built-in HTTP client. Again, just create a new file with the `.rest` extension and the editor will display your options.

##### Receiving Data #####
___
Now let's make it possible to add notes to the server.

Adding a note takes place when a HTTP POST request is sent to http://localhost:3001/api/notes and by sending all the information for the new note in the reauest body in JSON format.

We need json-parser to to access the data easily. 

You can activate this as so:

`app.use(express.json())`

Add the json-parser and post handler:

```js
const express = require('express')
const app = express()

app.use(express.json())

//...

app.post('/api/notes', (request, response) => {
  const note = request.body
  console.log(note)
  response.json(note)
})
```

We get the json data from the request's body property. But we need to parse it using json-parser to transform the json data to a JavaScript object. You don't have to explicitly code the parsing, just having that json line in your code is enough.

Use postman to check if your code works. POST a json and it should be logged to the console and sent back as a HTTP response.

Make sure Postman is sending the correct type of data (e.g. json). Check the content-type in the headers tab.

Or you can use the REST plugin for VSCode:

```rest
GET http://localhost:3001/api/notes/

###
POST http://localhost:3001/api/notes
content-type: application/json

{
    "content": "postman is cool",
    "important": true
}

```
*Separate requests with `###`*

Postman allows you to save requests

	> **Important sidenote**
> 
> Sometimes when you're debugging, you may want to find out what headers have been set in the HTTP request. One way of accomplishing this is through the [get](http://expressjs.com/en/4x/api.html#req.get) method of the _request_ object, that can be used for getting the value of a single header. The _request_ object also has the _headers_ property, that contains all of the headers of a specific request.

> Problems can occur with the VS REST client if you accidentally add an empty line between the top row and the row specifying the HTTP headers. In this situation, the REST client interprets this to mean that all headers are left empty, which leads to the backend server not knowing that the data it has received is in the JSON format.

>[!warning]
>Basically, your HTTP request and your content-type, or other header line, should not have a space between them (in a `.rest` file)

Let's finish up the POST request.

```js
app.post('/api/notes', (request, response) => {
  const maxId = notes.length > 0
    ? Math.max(...notes.map(n => n.id)) 
    : 0

  const note = request.body
  note.id = maxId + 1

  notes = notes.concat(note)

  response.json(note)
})
```

Find the largest id number in th ecurrent list and assign it to the maxID variable.

The new id is maxID + 1.

This is a terrible method so we'll see a better way later.

Let's change things even more. After this, the content property can't be empty:

```js
const generateId = () => {
  const maxId = notes.length > 0
    ? Math.max(...notes.map(n => n.id))
    : 0
  return maxId + 1
}

app.post('/api/notes', (request, response) => {
  const body = request.body

  if (!body.content) {
    return response.status(400).json({ 
      error: 'content missing' 
    })
  }

  const note = {
    content: body.content,
    important: body.important || false,
    id: generateId(),
  }

  notes = notes.concat(note)

  response.json(note)
})
```

We've also separated the id generation process into a new function.

##### HTTP Request Types #####
___
Two properties
1. *Safety*
2. *Idempotency*

GET requests should be safe. They should only retrieve information.

Safety - executing a safe request does not change the server in any way.

HEAD requests should also be safe. They only return the status code and response headers.

All requests bar POST should be idempotent. 

Idempotent requests have a single side-effect no matter how many times that request is sent.

Neither property is guaranteed, but both are recommended. When adhering to RESTful principles, GET, HEAD, PUT, and DELETE are idempotent.

POST is neither safe nor idempotent. Sending the same request multiple times results in the execution of back-end code 5 separate times.

##### Middleware ######
___
The express json-parser we have used is a so-called middleware.

Middleware are functions that are used in the process of handling request and response objects.

Json-parser takes raw data from the request object and parses it into a JavaScript objec as a new property `body`.

You can use multiple pieces of middleware at any one point in time.

They are executed one by one in the order they were taken into use by express.

`app.use(<some middleware>)`

We can add our own middleware that print information about every request sent to the server.

```js
const requestLogger = (request, response, next) => {
  console.log('Method:', request.method)
  console.log('Path:  ', request.path)
  console.log('Body:  ', request.body)
  console.log('---')
  next()
}
```

Middleware needs to be taken into use before routes if we want them to be executes before the route event handlers are called. 

You can define middleware after routes. These will only be used if the HTTP request is to a non-existent route.

For example:

```js
const unknownEndpoint = (request, response) => {
  response.status(404).send({ error: 'unknown endpoint' })
}

app.use(unknownEndpoint)
```

This will return a JSON format error message in the event that the browser sends a http request to a useless route.

