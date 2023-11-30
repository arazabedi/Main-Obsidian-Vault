https://www.reddit.com/r/nextjs/comments/vf1s9t/can_anyone_tell_what_exactly_is_next_js_as_the/

React itself is just a library without router, module bundler or server etc...

If you want to create react app without installing tons of packages/modules you can just create it with create-react-app but it's just frontend framework without backend server, router and etc..

Next.js solves this and it's probably new standard for making React applications cuz it's the simplest way without tons of thinking about which packages will you use.... Next.js adds very good router, image and link components, backend server with serverless functions and many more..

I hope I'm correct :)

Nextjs is a full stack framework:
- For **front end** it implements react code and turns it into magic with server side rendering, incremental static regeneration etc. ( boosting the seo performance). 
- For **routing** nextjs uses filesystem as routes, and for backend, nextjs uses nodejs in pages/api directory and subdirectories. 
- Other than that nextjs has some components that **optimizes** the end product, like next/image or next/script. 
- Also nextjs supports **sass** and **typescript** very easily, all happens automagically. 
- When you add prisma orm and link a mongodb server to it, it literally becomes the ERN in MERN stack.

NextJS essentially has three components:
- A server that renders React apps
- A compiler that bundles your FE and BE code
- A CLI tool to manage all these tasks for you.

Traditionally you would have to install a number of libraries to get the functionality of NextJs:
- react  
- reactRouter  
- reactHelmet  
- gatsby  
- reactQuery  
- ssr/isr/hybric of any  
- serverlessFunctions

React is the major part of the NextJS package, but Next is a toolset with much more functionality than React on its own.