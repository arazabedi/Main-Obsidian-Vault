https://www.youtube.com/watch?v=zb3Qk8SG5Ms&list=PL4cUxeGkcC9jsz4LDYc6kv3ymONOKxwBU

## Introduction and Setup 
___
Node.js is a programme written in c++ that wraps Google's V8 JavaScript engine (a compiler).

It also hooks into the V8 engine to add functionality to JavaScript such as:
- Read and write files on a computer
- Connect toa database
- Act as a server for content

In Node, you no longer have access to the DOM API so you cannot manipulate the front end.

The role of node is to run JavaScript on the back-end, or *server-side*, handling requests from the browser.

![[Pasted image 20230207195657.png]]

Node is an alternative to Java (Spring), Ruby (Rails), Python (Django), C# (.NET) etc.

There are benefits to using Node over its competitors:

1. Same language as the front-end
2. You can share code! between the front-end and back-end
3. Node.js has a massive community
4. Loads of third-party packages and tools

Course Overview:
1. Install Node & use it to run JavaScript
2. How to read & write files
3. How to create a server using Node.js to create a website
4. How to create an Express app / website
5. How to use MongoDB (a NoSQL database)
6. How to use template engines to easily create HTML views
7. Put everything together to make a simple blog site

Run a JavaScript file:

```bash
node <filename.js>
```

## Node.js Basics
___
**Global Object**
In node, we have access to the *global* object. This is a bit like the *window* object in the browser. These both have methods such as `setTimeout` and `alert` built in.

In the browser, you don't have to explicitly type:
```js
window.setTimeout
```
This is already implied in:
```js
setTimeout
```

In Node, the global object is `global`.

The following code: 
1. Displays the global object and its properties and methods
2. Displays the directory name and file name
3. Logs 'in the interval' every 100 ms
4. Lofgs 'in the timeout' after 500 ms and stops `int` from logging

```js
console.log(global);

setTimeout(() => {
	console.log('in the timeout');
	clearInterval(int);
}, 500)

const int = setInterval(() => {
	console.log('in the interval');
}, 100)

console.log(__dirname);
console.log(__filename);
```

![[Pasted image 20230207235608.png]]

Modules and Require
___
In order to run a separate file from a particular file, we create a constant variable and assign it the function `require` with an argument of the path of the file that we want to run:

```js
const xyz = require('./people');
```

>[!warning]
>This *only* runs that file, it does not give you access to its data or functions like `import React from 'react'` 

In order to use the data (variables, functions etc.) in another file, we have to `export`:

```js
module.exports = 'hello'
```

This will assign the string 'hello' the variable `xyz` (in the previous code block).

Now we can use the data in one file inside another.

We can also export multiple pieces of data to a variable in another file by using an object:

```js
// people.js
const people = ['yoshi', 'ryu', 'chun-li', 'mario'];
const ages = [20, 25, 30, 35]

console.log(people);

module.exports = {
	people: people,
	ages: ages
}
```

The export function can also be written as the following, because the properties and values have the same name:

```js
module.exports = {
	people, 
	ages
}
```

The other file would look like this:

```js
// modules.js
const xyz = require('./people');

console.log(xyz.people, xyz.ages);
```

You can use destructuring to import data from another file:

```js
/// modules.js
const {people, ages} = require('./people');

console.log(people, ages);
```

Node.js has some built-in modules.

The following code requires (imports) Node's OS module that has such methods as `platform()` (returns the OS name), and `homedir()`, which returns the home directory:

```js
const os = require('os');
console.log(os.platform(), os.homedir());
```

See here for more Node modules (APIs)
https://nodejs.org/api/

The File System
___
We can read, write, and delete files as well as create directories, and more.

First you have to require the `fs` (file system) module.

```js
const fs = require('fs')
```

Reading
___
`fs.readFile(<path>, (err, data) => {<callbackFunction})`

```js
// reading files
// This code will return (in order):
//last line
//<Buffer 68 65 6c 6c 6f 2c 20 6e 69 6e 6a 61 73 0a>

fs.readFile('./docs/blog.txt', (err, data) => {
	if (err) {
		console.log(err);
	}
	console.log(data.toString());
})

console.log('last line');
```

A buffer is a package of data sent to us when we read a file. You can us `data.toString()` in order to get the string instead of the buffer.

The second argument, which is an asynchronous callback function (which takes some time to execute), does not stop the code from carrying on and executing further down.

Therefore, the `console.log` at the bottom of the code actually logs first before the one above it.

Writing
___
`fs.writeFile(<path>, <overwriteText>, () => {callbackFunction})`

```js
// writing files

fs.writeFile('./docs/blog.txt', 'hello, world', () => {
	console.log('file was written');
})
```

If the file in the filepath doesn't exist, then node will create that file for you.

Directories
___
`fs.mkdir(<path>, (err) => {})`

```js
//directories

if(!fs.existsSync('./assets')) {
fs.mkdir('./assets', (err) => {
	if (err) {
		console.log(err);
	}
	console.log('folder created');
});
}
```

We've added an if statement to check that the folder doesn't already exist, as there would otherwise be an error.

The `existsSync()` function is a synchronous function so it will halt execution until it resolves.

Again, `fs.mkdir()` is an asynchronous function, and it will take time.

We can also add code so that if the folder already exists then it will be deleted:

`fs.rmdir(<path>, () => {callbackFunction})`

```js
//directories

if(!fs.existsSync('./assets')) {
fs.mkdir('./assets', (err) => {
	if (err) {
		console.log(err);
	}
	console.log('folder created');
});
} else {
	fs.rmdir('./assets', (err) => {
		if (err) {
		console.log(err);
		}
		console.log('folder deleted');
	})
}
```

Deleting
___
`fs.unlink(<path>, () => {callbackFunction})`

```js
// deleting files

if (fs.existsSync('./docs/deleteme.txt')) {
	fs.unlink('./docs/deleteme.txt', (err) => {
		if (err) {
			console.log(err);
		}
		console.log('file deleted');
	})
}
```

Streams and Buffers
___
https://nodesource.com/blog/understanding-streams-in-nodejs/

Streams - Start using data before it has finished loading

There are *read streams*, and *write streams*.

readStream
___
Here are the building blocks:

```js
const fs = require('fs')

const readStream = fs.createReadStream('./docs/blog3.txt')

readStream.on('data', (chunk) => {

})
```

`.on()` is an event listener, and 'data' is the data event. Every time we get a new 'chunk' of data, we fire the callback function (and we have access to that chunk of data).

Here is the complete package:

```js
const fs = require('fs')

const readStream = fs.createReadStream('./docs/blog3.txt', { encoding: 'utf8'})

readStream.on('data', (chunk) => {
	console.log('-------- NEW CHUNK ---------');
	console.log(chunk);
})

```

The second argument of the `createReadStream` function gives the ecoding property a value of utf-8, so the buffers that get sent to us will automatically be decoded to strings (this avoids the need for `.toString`).

writeStream
___

```js
const fs = require('fs')

const readStream = fs.createReadStream('./docs/blog3.txt', { encoding: 'utf8'})
const writeStream = fs.createWriteStream('./docs/blog4.txt')

readStream.on('data', (chunk) => {
	console.log('-------- NEW CHUNK ---------');
	console.log(chunk);
	writeStream.write('\nNEW CHUNK\n')
	writeStream.write(chunk);
})
```

Pipes
___
This works well when passing data directly from a readable to a writeable stream. It shortens the above code.

>[!warning]
>Pipes can only be used when passing data from a read stream to a write stream

```js
const fs = require('fs')

const readStream = fs.createReadStream('./docs/blog3.txt', { encoding: 'utf8'})
const writeStream = fs.createWriteStream('./docs/blog4.txt')

readStream.pipe(writeStream)
```

## Clients & Servers
___
We type a website address into a browser and hit enter. This send a request to the server. The server then uses that request to decide what to send to the browser as a response.

How does the browser know which server to send a request to?

IP Addresses & Domains
___
All computers connected to the internet have a unique IP addresses.

Host computers also have an IP address to identify it.

To connect to a server on a host computer, we need its IP address.

We can type the IP address into the browser in order to connect to a server.

Domain names are used to mask IP addresses. Browsers find the IP address associated with a domain.

![[Pasted image 20230208020623.png]]

When we type into the browser and hit enter, we send a GET request. This communication is via HTTP - a set of instructions determining how communication should occur (essentiallly the language of communication between web clients and web servers).

Here's how you create a server:

```js
const http = require('http')

const server = http.createServer((req, res) => {
	console.log('request made');
});

server.listen(3000, 'localhost', () => {
	console.log('listening for requests on port 3000');
})

```

We have required the http API, and assigned a new server to a new variable. The `createServer` function contains a callback function as an argument, which has the request, and response parameters. You can use either or both of these in the callback function for whatever purpose.

When a request is made, the callback is fired.

The `listen()` function decides which host, and which port number, the server is listening in on.

Localhosts and Port Numbers
___
What is localhost?

It's like a domain name on the web, but it sends you to to a loopback IP address (127.0.0.1) - the IP of your own computer.

![[Pasted image 20230208023319.png]]

What is a port number?

It's like a channel/gateway/do.

Any software that connects to the internet and sends or recieves data will be doing so via separate port numbers in order to keep data separate from each other.

Our server also needs a port number to communicate with the browser.

Web developers tend to use 3000.

By typing `localhost:3000` into the browser, you're telling the browser to connect to the computer it is running on, at the port 3000.

## Requests and Responses
___
The *Request Object* contains information about the request that the user sends.

For example:

`req.url` - returns everything after `/`
`req.method` - returns the HTTP method

Here is our code:

```js
const http = require('http')

const server = http.createServer((req, res) => {
	console.log(req.url, req.method);
});

server.listen(3000, 'localhost', () => {
	console.log('listening for requests on port 3000');
})
```

When the user goes to a page on our domain (in this case `localhost:3000`), a request is sent to our server, which we can return using `req.something`.

Response Object
___
The first step in formulating a response is to determine the response headers. 

Headers tell the browser what sort of data is being sent to it, usually HTML, CSS, JSON etc.

You can also use headers to set cookies.

```js
// set header content type
res.setHeader('Content-Type', 'text/plain');
```

Nex, we write whatever content we would like to send:

```js
res.write('hello, ninjas');
```

And finally we have to end the response and send it:

```js
res.end();
```

All of this goes in the server callback function.

What if you wanted to send HTML?

```js
const http = require('http')

const server = http.createServer((req, res) => {
	console.log(req.url, req.method);

	//set header content type
	res.setHeader('content-Type', 'text/html');
	
	res.write('<head><link rel="stylesheet" href="#"</head>');
	res.write('<p>hello, ninjas</p>');
	res.write('<p>hello again, ninjas</p>');
	res.end();
});

server.listen(3000, 'localhost', () => {
	console.log('listening for requests on port 3000');
})
```

What about a file?

Returning HTML Pages
___
All you have to do is to use the `fs` module to read a file in your server directory, and in its callback function use the `res.write()` function.

Remember that the conditional is important in order to avoid hanging.

```js
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
	console.log(req.url, req.method);

	//set header content type
	res.setHeader('content-Type', 'text/html');

	//send an html file
	fs.readFile('./views/index.html', (err, data) => {
		if (err) {
			console.log(err);
			res.end();
		} else {
			res.write(data);
			res.end();
		}
	})
});

server.listen(3000, 'localhost', () => {
	console.log('listening for requests on port 3000');
})
```

We can also send data directly with the `end` method:

```js
res.end(data);
```

Basic Routing
___
Create a path variable that is  linked to your views directory. Use a `switch` statement to go through each different url you wish to use for routing, and add the specific file path to you path variable for each one.

Then, in your readFile funtion, simply read this dynamic `path` variable whenever a request is sent, as a response.

```js
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
	console.log(req.url, req.method);

	//set header content type
	res.setHeader('content-Type', 'text/html');

	let path = './views/';
	switch(req.url) {
		case '/':
			path += 'index.html';
			break;
		case '/about':
			path += 'about.html';
			break;
		default:
			path += '404.html';
			break;
	}

	//send an html file
	fs.readFile(path, (err, data) => {
		if (err) {
			console.log(err);
			res.end();
		} else {
			// res.write(data);
			res.end(data);
		}
	})
});

server.listen(3000, 'localhost', () => {
	console.log('listening for requests on port 3000');
})
```

Status Codes
___
Status codes describe the response sent to the browser, and how succesful the request was.

*Common*
200 - A-okay
301 - Resource moves (permanent redirect)
404 - File not found
500 - Internal server error

*Ranges*
100 Range - informational responses
200 Range - success codes
300 Range - codes for redirects
400 Range - user or client error codes
500 Range - server error codes

Let's now add status codes to our response objects.

Just it to the `switch` statements.

```js
const http = require('http');
const fs = require('fs');

const server = http.createServer((req, res) => {
	console.log(req.url, req.method);

	//set header content type
	res.setHeader('content-Type', 'text/html');

	let path = './views/';
	switch(req.url) {
		case '/':
			path += 'index.html';
			res.statusCode = 200;
			break;
		case '/about':
			path += 'about.html';
			res.statusCode = 200;
			break;
		default:
			path += '404.html';
			res.statusCode = 404;
			break;
	}

	//send an html file
	fs.readFile(path, (err, data) => {
		if (err) {
			console.log(err);
			res.end();
		} else {
			// res.write(data);
			res.end(data);
		}
	})
});

server.listen(3000, 'localhost', () => {
	console.log('listening for requests on port 3000');
})
```

Redirects
___
If a url is changed, we need to redirect.

Here, we are directing `/about-me` to `/about` using headers (`setHeader`)

```js
	let path = './views/';
	switch(req.url) {
		case '/':
			path += 'index.html';
			res.statusCode = 200;
			break;
		case '/about':
			path += 'about.html';
			res.statusCode = 200;
			break;
		case '/about-me':
			res.statusCode = 301;
			res.setHeader('Location', '/about');
			res.end();
		default:
			path += '404.html';
			res.statusCode = 404;
			break;
	}
```

![[Pasted image 20230208140045.png]]

When our website gets bigger, this way of routing will get messy. There is a third-party package called Express.js that will make things cleaner.

But first. NPM.

## NPM
___
Nodemon is an NPM package that allows for the live reload of your server.

Instead of using `node <file>` to run a file with a server function, use:

```bash
nodemon <file>
```

The package.json file in your project directory keeps track of any dependencies you have for your project.

In your command line, type in:

```bash
npm init
```

This will create a `package.json`.

You also need to get a `package-lock.json` which will keep track of package (dependency) versions:

```bash
npm install
```

We will also be using *lodash*, a utility package that contains functions for things such as:

`random(min, max)` - return a random number between the two arguments.

`once(<callback>)` - Any function inside this one can only be called once on each execution of the file.

Remember to `const _ = require('lodash')` at the top of your file.

Dependencies
___
NPM easily allows us to share project code.

Sometimes the `node_modules` folder is huge, so you wouldn't share it. 

Instead, the person you're sharing the project with would just use `npm install` in their cli, which will use your `package.json` to figure out what to install on their computer.

NPM will look at the dependencies section of the JSPN and install everything there.

```json
{
  "name": "blog-app",
  "version": "1.0.0",
  "description": "",
  "main": "server.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "node server.js"
  },
  "author": "",
  "license": "ISC",
  "dependencies": {
    "lodash": "^4.17.21"
  }
}
```

Now we can start learning about the Express.js framework.

## Express Apps
___
The rudimentary way of routing using switch can get messy very quickly in a large project.

Express.js handles requests, server-side logic, and routing in a much more elegant way.

Create an instance of an Express app:
```js
const express = require('express');

// express app
const app = express();
```

Listen for a request and create an instance of the server:
```js
app.listen(3000);
```

Listen for a GET request to the root (infers type, sets staus code, and sets headers automatically):

```js
app.get('/', (req, res) => {

	res.send('<p>home page</p>');
})
```

Routing and HTML
___
How to send a file:

```js
app.get('/', (req, res) => {
	// res.send('<p>home page</p>');
	res.sendFile('./views/index.html', { root: __dirname });
})
```

The second argument is important as Express.js looks for the path from the root directory of the computer. By setting the root to the directory with `{ root: __dirname}`, you avoid having to list every folder in the first argument.

There is also a `path` module in  which allows you to join together the directory name with the filepath, but the option we've used may be cleaner.

Redirects and 404s
___
**How to redirect:**

```js
app.get('/about-us', (req, res) => {
	res.redirect('/about')
})
```

This will redirect an `/about-us` GET request to the `/about` page.

`.use` is used to fire middleware. We will see more of it later.

**How do you route 404 pages?**

```js
app.use((req, res) => {
	res.status(404).sendFile('./views/404.html', { root: __dirname });
})
```

*This **MUST** go at the bottom of your routing functions because it will fire for any HTTP request.*

Express checks HTTP requests against `.get` functions in sequential order, and stop checking once it finds a match. Therefore there can only be one response (thought that response can contain multiple sources and types of data).

This means that you must **ALWAYS** put your 404 router function beneath your other router functions.

`use` fires for every single request if the code reaches that point.

We must also add the `.status(404)` function as `use()` does not handle this itself. We can append the `sendFile()` function to this as well becuse the status function simply returns the response object with a new status header.

Here is the full code:

```js
const express = require('express');

// express app
const app = express();

// listen for requests (and create an instance of the server)
app.listen(3000);

// listen for a GET request to the root (infers type, sets staus code, and sets headers automatically)
app.get('/', (req, res) => {
	// res.send('<p>home page</p>');
	res.sendFile('./views/index.html', { root: __dirname });
})

app.get('/about', (req, res) => {
	res.sendFile('./views/about.html', { root: __dirname });
})

// redirects
app.get('/about-us', (req, res) => {
	res.redirect('/about')
})

// 404 page
app.use((req, res) => {
	res.status(404).sendFile('./views/404.html', { root: __dirname });
})
```

Much nicer than pure Node.

Now how do we inject dynamic data and contents into our different templates?

We use a view engine!

## View Engines
___
A view engine allows you to inject dynamic data into HTML, for example user data, or  a blog post, from a database.

Express.js can use view engines very easily.

There are a few different options:
- Express Handlebars
- Pug
- EJS

We will be using EJS.

```bash
npm install ejs
```

We have to register the view engine in our app.js (server) file:

```js
app.set('view engine', 'ejs');
```

Express.js and EJS automatically look at the `view` directory within your project director, but you can pick a different folder (`myviews` below):

```js
app.set('view', 'myviews');
```

These go just below your express app line:

```js
const express = require('express');

// express app
const app = express();

// register view engine
app.set('view engine', 'ejs');
app.set('view', 'myviews');

// listen for requests (and create an instance of the server)
app.listen(3000);

// listen for a GET request to the root (infers type, sets staus code, and sets headers automatically)
app.get('/', (req, res) => {
	// res.send('<p>home page</p>');
	res.sendFile('./views/index.html', { root: __dirname });
})

app.get('/about', (req, res) => {
	res.sendFile('./views/about.html', { root: __dirname });
})

// redirects
app.get('/about-us', (req, res) => {
	res.redirect('/about')
})

// 404 page
app.use((req, res) => {
	res.status(404).sendFile('./views/404.html', { root: __dirname });
})
```

Once you have set your view engine, create an `index.ejs` which will be the page where dynamic data will be displayed.

Once you have this, change you GET request routing so that instead of `.send()`, you are using `.render()`.

```js
app.get('/', (req, res) => {
	res.render('index');
})
```

in the above callback function, it is enough to write 'index' as the argument, as Express.js is automatically looking in the `view` directory.

Here is our app.js now:

```js
const express = require('express');

// express app
const app = express();

// register view engine
app.set('view engine', 'ejs');

// listen for requests (and create an instance of the server)
app.listen(3000);

// listen for a GET request to the root (infers type, sets staus code, and sets headers automatically)
app.get('/', (req, res) => {
	res.render('index');
})

app.get('/about', (req, res) => {
	res.render('about');
})

app.get('/blogs/create', (req, res) => {
	res.render('create');
})

// 404 page
app.use((req, res) => {
	res.status(404).render('404');
})
```

Passing Data into Views
___
```js
<% %>
```

Huuuuuhhhh?

Yup. It;s ERB. But for JavaScript.

Anything inside the templates will run on the *server*, not the *client*.

In order to render on the client, use:

```js
<%= %>
```

Just like ERB.

:().

You can pass data from the server to the browser using objects.

Just add a second argument to the render method:

```js
//app.js
app.get('/', (req, res) => {
	res.render('index', { title: 'Home'});
})
```

Then just use the *equals template* in the ejs (basically html) file:

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<meta http-equiv="X-UA-Compatible" content="IE=edge">
	<meta name="viewport" content="width=device-width, initial-scale=1.0">
	<title>Blog Ninja | <%= title %></title>
</head>
```

>[!info]
>The title element inside the head is what the browser uses to display text information on the tab.

![[Pasted image 20230208175107.png]]

 We can also pass in arrays and objects.

Create a variable containing an array of objects, then use a loop to display them in your view.

```js
//app.js
...
app.get('/', (req, res) => {
  const blogs = [
    {title: 'Yoshi finds eggs', snippet: 'Lorem ipsum dolor sit amet consectetur'},
    {title: 'Mario finds stars', snippet: 'Lorem ipsum dolor sit amet consectetur'},
    {title: 'How to defeat bowser', snippet: 'Lorem ipsum dolor sit amet consectetur'},
  ];
  res.render('index', { title: 'Home', blogs });
});
...
```

We are sending the `blogs` object to the view by putting it inside the second argument of the `render` method.

We have but the array `blogs`, which contains multiple objects, inside an object which is read by ejs when `blogs` is encountered within a template inside an ejs file.

```js
<% if (blogs.length > 0 ){ %>
	<% blogs.forEach(blog => { %>
		<h3 class="title"><%= blog.title %></h3>
		<p class="snippet"><%= blog.snippet %></p>
		<% }) %>
<% } else { %>
	<p>There are no blogs to display</p>
<% } %>
```

EJS Templates are processed throught the EJS view engine on the server.

Our view files live on our server, and when we want to render one to the browser, it gets sent to the EJS view engine to be processed.

The EJS engine looks for any dynamic content (templated js), and figures out the html corresponding to the javascript in the templates,  then spits out a valid html page based on the template we wrote. This is then sent to the browser.

This process is called Server-Side Rendering (SSR).

Partials
___
Just like in rails, you can put html into different components, called *partials*, to be imported into our views.

Create a file called partials.

Then make your ejs files.

This is how you inject your `nav.ejs` partial into your `index.ejs` file.

```html
//index.ejs
<%- include('./partials/nav.ejs')  %>
```

*Note the hyphen*. This is because the equals sign escapes special characters, and we end up witha string value instead of raw html.  By using the hyphen, the html will be copied over exactly.

## Middlewear and Static Files
___
Middlewear is any code which runs on the server between getting the request and sending the response.

The `use()` method is used to run middlewear code.

The functions that run in our `get()` handlers are also middlewear.

>[!info]
>The difference between the `get()` and `use()` methods:
>- `get()` - runs only for `GET` requests, and also only to a specific route
>- `use()` runs for all http requests to all routes

Middlewear runs from top to bottom, and ends when a response is sent to the browser.

![[Pasted image 20230209011427.png]]

![[Pasted image 20230209011451.png]]

Using next()
___
You have to explicitly tell Express.js to move on after a `use()` function by invoking the `next()` function.

This tells Express.js that we're done with the middleware and that it can move on to the next one.

```js
app.use((req, res, next) => {
  console.log('new request made:');
  console.log('host: ', req.hostname);
  console.log('path: ', req.path);
  console.log('method: ', req.method);
  next();
});
```

if you don't add `next()` then the page will hang.

Third-party Middleware
___
- *Morgan* - logger (better than `console.log()`)
- *Helmet* - security
- Sessions
- Cookies
- Validation
- Many more...

We are going to be using Morgan, so first install it:
```bash
npm install morgan
```

Require it:
```js
const morgan = require('morgan');
```

Invoke the `morgan()` function:
```js
app.use(morgan('dev'));
```

This will give you:
![[Pasted image 20230209134621.png]]

Explanation of the Morgan log:
```
:method :url :status :response-time ms - :res[content-length]
```

There are other arguments you can find here:

https://www.npmjs.com/package/morgan

Static Files
___
If we added static files to the project directory such as images or css files, we wouldn't be able to access them from the browser as is, even if you place a link to them in the head of your ejs file:

![[Pasted image 20230209134904.png]]

This is done to protect your files from malicious users.

To give the client access to specific static files, you need to make them public using a built in Express.js method `static()`.

```js
app.use(express.static('public'))
```

This is the path you should be using:
```html
<link rel="stylesheet" href="/styles.css" />
```

>[!warning]
>The following will not work as Express.js `static()` determines the root of any links in ejs files.
>```html
><link rel="stylesheet" href="../../public/styles.css" />
>```

If we have specified a particular folder as public, then take that folder to be the root when it comes to your html/ejs.

## MongoDB
___
We'll be using a mongoDB database for this project.

![[Pasted image 20230209141858.png]]

In mongoDB, we have collections (folders), and documents. 

Each collection is used to store a particular type of data, for example user data, or blog documents.

![[Pasted image 20230209142008.png]]

A document represents a single item of data. In mongoDB they are very similar to JSON, containing key-value pairs, and every document contains a unique id key.

We're going to be using a cloud service called MongoDB Atlas.

I've made an Atlas account, added a database admin with username:
arazabedi and the usual unsophisticated password.

Set up a database and go through the site to find a connect to your database option.

Find a connect to your app option.

Won't go in detail because other services will have different layouts.

The databse service will give you a URI. Chuck it into a variable in your `app.js` file.

```js
// connect to mongoDB
const dbURI = 'mongodb+srv://arazabedi:<password>@nodetuts.d9chpmj.mongodb.net/?retryWrites=true&w=majority'
```
*Not this one obviously. Change the `<password>` to your actual password as well.*

There is a plain mongoDB API for interacting with your database.

But.

Mongoose is better.

Mongoose, Models, & Schemas
___
Mongoose is an ODM library - Object Document Mapping.

This is like active record, which is an ORM (Object Relational Mapping), except that mongoDB is a document-based database and not a relational one.

https://medium.com/@julianam.tyler/what-is-the-difference-between-odm-and-orm-267bbb7778b0#:~:text=The%20main%20difference%20is%20that,sheet%2C%20with%20rows%20and%20columns

![[Pasted image 20230209143750.png]]

Mongoose wraps the standard MongoDB API, and makes it easier for us to interact with our database.

It does this by allowing us to create simple data models which have database methods with CRUD functions.

Mongoose queries the correct database collection based on the model we use, and returns a response.

Mongoose isn't necessary, but the standard MongoDB library is more clunky and verbpse.

We need to create schemas and models.

In Mongoose, we create a schema first.

A schema defines the structure of a data-type or document stored in a database collection.

A blog schema for example would have:
- id (auto-generated)
- title(string), required
- snippet (string), (require)
- body (string), required

The model allows us to communicated with the database.

The Blog Model will have static and instance methods to do CRUD actions on the database.

```bash
npm install mongoose
```

Connect to the database
```js
mongoose.set('strictQuery', false).connect(dbURI);
```
*the set function is to avoid a cli warning*

This is an asynchronous task, so it takes some time to do, and this means that you can tack on a `then()` method to it:

```js
mongoose.set('strictQuery', false).connect(dbURI)
	.then((result) => console.log('connected to db'))
	.catch((err) => console.log(err));
```

We shouldn't be listening for http requests until the database is connected:

```js
// connect to mongoDB
// listen for requests (and create an instance of the server)
const dbURI = 'mongodb+srv://arazabedi:cavendish@nodetuts.d9chpmj.mongodb.net/?retryWrites=true&w=majority'
mongoose.set('strictQuery', false).connect(dbURI)
	.then((result) => app.listen(3000))
	.catch((err) => console.log(err));
```

Basically `.then()` the `app.listen()` creation of the server and begin listening - only does this once the database has been connected.

Creating a Model
___
Create a `model` folder.

Create a new js file with your model name, in this case, `blog.js`.

Set a constructor function to a constant:

```js
const Schema = mongoose.Schema;
```

Invoke the constructor function:

```js
const blogSchema = new Schema({
	
})
```

Add the properties and define their data-types:

```js
const blogSchemas = new Schema({
	title: String,
	snippet: String,
	body: String
})
```

We also want to add extra validation so open up objects for each property to give it more specificity:

```js
const blogSchema = new Schema({
	title: {
		type: String,
		required: true
	},
	snippet: {
		type: String,
		required: true
	},
	body: {
		type: String,
		required: true
	}
})
```

You can add a second argument, an object, to add options:

```js
const blogSchema = new Schema({
	title: {
		type: String,
		required: true
	},
	snippet: {
		type: String,
		required: true
	},
	body: {
		type: String,
		required: true
	}
}, { timestamps: true })
```

*We've added a timestamp feature for CRUD actions*

Now, we need to create a model based on the schema.

The model surrounds the schema and provides an interface for interaction with each document type.

```js
const Blog = mongoose.model('Blog', blogSchema)

module.export = Blog
```

*Note that the first argument is the singular of the collection name, and the second argument is the schema name*

Don't forget to export. 

Require Blog in `app.js`

```js
const blog = require('./models/blog')
```
*I know it's not capitlised, don't ask me why. Giving Rails right now.*

Assign the database connection URI to a new variable:

```js
const dbURI = 'mongodb+srv://arazabedi:cavendish@nodetuts.d9chpmj.mongodb.net/?retryWrites=true&w=majority'
```

Connect to the database and listen for requests (move the `app.listen(3000)` into this `.connect()` sequence). The task is asynchronous, so you can do this as a `.then()`.

```js
mongoose.set('strictQuery', false).connect(dbURI, { useNewUrlParser: true, useUnifiedTopology: true })

.then(result => app.listen(3000))
.catch(err => console.log(err));
```

Create your routes:

```js
// mongoose and mongo sandbox routes
app.get('/add-blog', (req, res) => {
	const blog = new Blog({
		title: 'new blog',
		snippet: 'about my new blog',
		body: 'more about my new blog.',
	})

	blog.save()
		.then((result) => {
			res.send(result);
		})
		.catch((err) => {
			console.log(err);
		})
})
```

This says that on a GET request to the /add-blog path, the server will create a Blog document and assign it to the constant `blog`. Then the server will save that document to the blogs collection, after which the server will send the result of the `save()` function (the document itself) to the browser, or log an error if no document has been made.

This will get all the blog documents and display it in the browser when it goes to the /all-blogs url.

```js
app.get('/all-blogs', (req, res) => {
	Blog.find()
	.then((result) => {
		res.send(result);
	})
	.catch((err) => {
		console.log(err);
	})
})
```

>[!warning]
>Note that on saving a blog document, we use the `.save()` *instance* method. When viewing all the blogs we are using a method on the collection as a whole (Blog) - which is capitalised.

If we want a single blog:

```js
app.get('/single-blog', (req, res) => {
	Blog.findById('63e516db8b5e40b649316d74')
	.then((result) => {
		res.send(result);
	})
	.catch((err) => {
		console.log(err);
	})
})
```

Outputting Documents in Views
___
SImply pass the variables you want your views to use (kind of like props in React) by adding them into the object that is the second argument of your `render()` function.

```js
// blog routes
app.get('/blogs', (req, res) => {
	Blog.find().sort({ createdAt: -1 } )
	.then((result) => {
		res.render('index', { title: 'All Blogs', blogs: result})
	})
	.catch((err) => {
		console.log(err);
	})
})
```

*We are also sorting the documents based on their timestamps - { `createdAt: -1 }`. The -1 is so that the newest goes first.*

## Get, Post, and Delete Requests
___

