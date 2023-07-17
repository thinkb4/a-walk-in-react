# [A walk in React](/README.md)

## DAY 9
- [DAY 9](#day-9)
    - Testing React apps (Unit tests)
        - Different kinds of tests
        - Tools
        - Writing our first test
        - Working with mocks
    - Deploying React apps
        - Steps
        - Lazy loading
        - Server side routing


## Deploying React apps

Up until this moment we have been writing code and testing our application locally. In order to share our aplication we will need to follow some steps to *deploy* it.

### Steps
    1. Build the Application: Once your code is thorroughly tested you will need to execute a script that wil create a bundle of our proyect which is compressed as posible. 
    2. Deploy: Upload production code to server
    3. 



### Lazy loading

Before we build our app for deployment we could explore ways of optimizing our code. As we've seen previously we must import the components we are using for them to work, import connects the different files, and all imported files must be loaded in order for our component to render. This means that before the application is shown to end users, all the imports must be resolved previously to showing something on the screen. In a simple application, this poses no problem but in larger applications, it can definitely cause slow rendering or other performance issues. Lazy Loading can help with this.  

**Lazy loading** means that we will only run certain pieces of code when they are needed.
In order to add lazy loading we need to remove the simple import we've been using. Instead, we will import *lazy* function from react  and call it to declare a lazy-loaded component.

```javascript
import { lazy } from 'react';

const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));
```

The lazy function returns a React component that you can render in your component tree. It takes load as parameter wich is a function returns an Promise or another Promise like object with a *then* method. After the load method is called, React will wait for it to be resolved in order to render the component.

> *Note:* Both the returned Promise and the Promise’s resolved value will be cached, so React will not call load more than once. If the Promise rejects, React will throw the rejection reason for the nearest Error Boundary to handle.

React provides a *Suspense* component that we can use in order to indicate the user the content is loading. We can provide a fallback property to Suspense that we can use until the component is rendered.
You can using by wraping the lazy component or any of itrs parents with it.

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

### Server side routing