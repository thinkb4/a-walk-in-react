# [A walk in React](/README.md)

## DAY 9
- [DAY 9](#day-9)
    - Testing React apps (Unit tests)
        - Different kinds of tests
        - Tools
        - Writing our first test
        - Working with mocks
    - [Deploying React apps](#deploying-react-apps)
        - [Steps](#steps)
            1. [Test and optimize your code.](#1-test-and-optimize-your-code)
                - [Lazy loading](#lazy-loading)
            2. [Build the Application](#2-build-the-application)
            3. [Deploy to server](#3-deploy-to-server)

## Deploying React apps

Up until this moment we have been writing code and testing our application locally. In order to share our application we will need to follow some steps to *deploy* it.

### Steps

### 1. Test and optimize your code.

Before deploying our application we must thoroughly test it and optimize our code for better performance.
One of the ways we can do this is by applying *lazy loading* in certain components that may make our application slower. 


### Lazy loading

As we've seen previously we must import the components we are using for them to work, import connects the different files, and all imported files must be loaded in order for our component to render. This means that before the application is shown to end users, all the imports must be resolved previously to showing something on the screen. In a simple application, this poses no problem but in larger applications, it can cause slow rendering or other performance issues. [Lazy Loading](https://react.dev/reference/react/lazy) can help with this.  

**Lazy loading** means that we will only run certain pieces of code when they are needed.
To add lazy loading we need to remove the simple import we've been using. Instead, we will import *lazy* function from react and call it to declare a lazy-loaded component.

```javascript
import { lazy } from 'react';

const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));
```

The lazy function returns a React component that you can render in your component tree. It takes *load* as a parameter which is a function that returns a Promise or another Promise-like object with a *then* method. After the load method is called, React will wait for it to be resolved to render the component.

> *Note:* Both the returned Promise and the Promise’s resolved value will be cached, so React will not call load more than once. If the Promise rejects, React will throw the rejection reason for the nearest Error Boundary to handle.

React provides a [*Suspense*](https://react.dev/reference/react/Suspense) component that we can use to indicate to the user that the content is loading. We can provide a fallback property to Suspense that we can use until the component is rendered.
You can use it by wrapping the lazy component or any of it's parents with it.

````javascript
<Suspense fallback={<Loading />}>
  <h2>Preview</h2>
  <MarkdownPreview />
</Suspense>
````

Keep in mind you should not declare lazy components *inside* other components but rather at the top level of your module. 

````javascript
import { lazy } from 'react';

function Editor() {
  // 🔴 Bad: This will cause all state to be reset on re-renders
  const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));
  // ...
}
````
````javascript
import { lazy } from 'react';

// ✅ Good: Declare lazy components outside of your components
const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));

function Editor() {
  // ...
}
````

### 2. Build the Application

The code we have written is not the one that will be deployed. Our code is optimized for human readability and sometimes uses features not supported by browsers (like JSX code).  
Instead, we must execute a script *npm run build* that will create a bundle with a highly optimized and compressed version of our code, and store it in a **build** folder that will be created at the root of our project. And it's the contents of that folder that will be deployed to the server. 

### 3. Deploy to server

Our application is ready to be deployed. It is a static website that only contains HTML, CSS, and Javascript. No code will ran in the server but rather it will be parsed and executed by our visitor's browser.   
The React SPA only requires a static site host.
There are many providers and you can search them online. Typically they will provide you with the steps to upload and deploy your project.  
Here you'll find tutorials for deploying your app in [Firebase](https://create-react-app.dev/docs/deployment/#firebase) or [Github pages](https://create-react-app.dev/docs/deployment/#github-pages), or you can read [this tutorial](https://blog.logrocket.com/8-ways-deploy-react-app-free/) with different options for deploying. 
