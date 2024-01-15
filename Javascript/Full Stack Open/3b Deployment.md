##### Same Origin Policy and CORS #####
___

When you visit a website, the browser sends a request to the server hosting the website, which responds with an HTML file containing references to external resources. If the browser fetches these resources using a URL with a different origin than the source HTML, it checks the Access-Control-Allow-Origin response header. If it contains the URL of the source HTML or *, the browser processes the response; otherwise, it throws an error due to the same-origin policy, which is a security mechanism to prevent session hijacking and other vulnerabilities.

To allow legitimate cross-origin requests, W3C created the CORS mechanism, which lets web pages request restricted resources from another domain. However, by default, JavaScript code in a browser can only communicate with servers in the same origin. To allow requests from other origins, we can use Node's cors middleware.

*Cors install:*

```bash
npm install cors
```

*Middleware:*

```js
const cors = require('cors')

app.use(cors())
```

##### Deploying to the Internet #####
___
You can use Fly.io or Render. Heroku used to be the big free one but they ended that.

I'll be using Fly.io.

Change the code for the port in the Node server so it works with Fly.io.

```js
const PORT = process.env.PORT || 3001app.listen(PORT, () => {
  console.log(`Server running on port ${PORT}`)
})
```

Deploy (do this again to deploy to production):

```bash
fly deploy
```

Open in browser:

```bash
fly open
```

View server logs:
```bash
fly logs
```

Deploying a React App:
```bash
npm run build
```

Chuck your front-end build directory into your back-end directory.

To show static content, the page index.html and the JS etc. using express, we need a middleware called static.

```js
app.use(express.static('build'))
```

Whenever express gets an HTTP GET request it will first check if the _build_ directory contains a file corresponding to the request's address. If a correct file is found, express will return it.

Now HTTP GET requests to the address _www.serversaddress.com/index.html_ or _www.serversaddress.com_ will show the React frontend. GET requests to the address _www.serversaddress.com/api/notes_ will be handled by the backend's code.

Because of our situation, both the frontend and the backend are at the same address, we can declare _baseUrl_ as a [relative](https://www.w3.org/TR/WD-html40-970917/htmlweb.html#h-5.1.2) URL. This means we can leave out the part declaring the server.

```js
import axios from 'axios'
const baseUrl = '/api/notes'
const getAll = () => {
  const request = axios.get(baseUrl)
  return request.then(response => response.data)
}

// ...
```

Now create a new production build and copy it to the root of your back-end directory.

The app now works from localhost 3001.

Congrats on making your first single-page application.

Finally `fly deploy` it again.

##### Streamling Deployment of the Frontend #####
____
Chuck these into your frontend directory (not the build)

Fly.io Scripts:

```json
{
  "scripts": {
    // ...
    "build:ui": "rm -rf build && cd ../part2-notes/ && npm run build && cp -r build ../notes-backend",
    "deploy": "fly deploy",
    "deploy:full": "npm run build:ui && npm run deploy",    
    "logs:prod": "fly logs"
  }
}
```

The script _npm run build:ui_ builds the frontend and copies the production version under the backend repository. _npm run deploy_ releases the current backend to Fly.io.

_npm run deploy:full_ combines these two scripts.

There is also a script _npm run logs:prod_ to show the Fly.io logs.

Note that the directory paths in the script _build:ui_ depend on the location of repositories in the file system.

Because in development mode the frontend is at the address _localhost:3000_, the requests to the backend go to the wrong address _localhost:3000/api/notes_. The backend is at _localhost:3001_.

If the project was created with create-react-app, this problem is easy to solve. It is enough to add the following declaration to the _package.json_ file of the frontend repository.
Add this to the frontend:

```json
{
  "dependencies": {
    // ...
  },
  "scripts": {
    // ...
  },
  "proxy": "http://localhost:3001"}
```

After a restart, the React development environment will work as a [proxy](https://create-react-app.dev/docs/proxying-api-requests-in-development/). If the React code does an HTTP request to a server address at _[http://localhost:3000](http://localhost:3000/)_ not managed by the React application itself (i.e. when requests are not about fetching the CSS or JavaScript of the application), the request will be redirected to the server at _[http://localhost:3001](http://localhost:3001/)_.

Now the frontend is also fine, working with the server both in development- and production mode.

A negative aspect of our approach is how complicated it is to deploy the frontend. Deploying a new version requires generating a new production build of the frontend and copying it to the backend repository. This makes creating an automated [deployment pipeline](https://martinfowler.com/bliki/DeploymentPipeline.html) more difficult. Deployment pipeline means an automated and controlled way to move the code from the computer of the developer through different tests and quality checks to the production environment. Building a deployment pipeline is the topic of [part 11](https://fullstackopen.com/en/part11) of this course.

There are multiple ways to achieve this (for example placing both backend and frontend code [in the same repository](https://github.com/mars/heroku-cra-node) ) but we will not go into those now.

In some situations, it may be sensible to deploy the frontend code as its own application. With apps created with create-react-app it is [straightforward](https://github.com/mars/create-react-app-buildpack).

The current backend code can be found on [Github](https://github.com/fullstack-hy2020/part3-notes-backend/tree/part3-3), in the branch _part3-3_. The changes in frontend code are in _part3-1_ branch of the [frontend repository](https://github.com/fullstack-hy2020/part2-notes/tree/part3-1).