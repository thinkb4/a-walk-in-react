# [A walk in React](/README.md)

## DAY 8

- [DAY 8](#day-8)
  - [Debugging React apps](#debugging-react-apps)
    - [React DevTools](#react-devtools)
  - [Testing React apps](#testing-react-apps)
    - [Different kinds of tests](#different-kinds-of-tests)
    - [Tools](#tools)
    - [Writing our first test](#writing-our-first-test)
    - [Testing Asynchronous Code](#testing-asynchronous-code)
    - [Working with mocks](#working-with-mocks)
  - [Deploying React apps](#deploying-react-apps)
    - [Steps](#steps)
      1. [Test and optimize your code](#1-test-and-optimize-your-code)
          - [Lazy loading](#lazy-loading)
      2. [Build the Application](#2-build-the-application)
      3. [Deploy to server](#3-deploy-to-server)

## Debugging React apps

There are many ways to debug a React app, since React is built with Javascript. Debugging helps you identify and fix issues in your code, ensuring your application runs smoothly. The most common way to debug a particular section of Javascript code is to use the browser's developer console.

React provides clear error messages in the browser's console when something goes wrong. These messages often include information about which component and line of code caused the error.

The simplest and most basic way to debug is by using `console.log()` statements to print values, objects, or messages to the console. You can place these statements in your component's functions to track the flow of data and state changes.

```javascript
function MyComponent() {
  const [count, setCount] = useState(0);

  const handleIncrement = () => {
    console.log("Increment button clicked");
    setCount(count + 1);
  };

  console.log("Render MyComponent with count:", count);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleIncrement}>Increment</button>
    </div>
  );
}
```

Also, you can use the `debugger` statement in your code to set **breakpoints**. When your application runs into a debugger statement, it will pause execution, allowing you to inspect variables and the call stack in the browser's developer tools.

```javascript
function MyComponent() {
  const [count, setCount] = useState(0);

  const handleIncrement = () => {
    debugger; // This will pause execution when clicked
    setCount(count + 1);
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleIncrement}>Increment</button>
    </div>
  );
}
```

### React DevTools

React DevTools is a browser extension available for Chrome and Firefox that provides a set of tools for inspecting and debugging React components. It allows you to inspect the component tree, view props and state, and even modify them in real-time. You can install React DevTools from the Chrome Web Store or Firefox Add-ons. To use it, open your app in the browser, right-click on an element, and select "Inspect" In the "Components" tab, you can navigate the component tree and inspect component details.

In addition to browser extensions, you can also use the standalone version of React Developer Tools. This tool works with any browser and allows you to inspect React components in a separate window. If you're using `Redux` for [state management](day_05.md#global-state-management), consider using the Redux DevTools extension. It allows you to inspect and debug your application's state changes and actions.

Remember that effective debugging often requires a combination of these techniques and tools. Additionally, keep an eye on your browser's developer console for error messages and warnings, as they can provide valuable insights into potential issues in your React application.

## Testing React apps

As we write our code we can run and preview our application. We get to see what the user will see and that allows us to make sure the app is running smoothly. This is called Manual testing and it is an important step in the development of our app. However, it can be error-prone since it's difficult to test all the different scenarios and options that may come up.  
[Automated testing](https://developer.mozilla.org/en-US/docs/Learn/Tools_and_testing/Cross_browser_testing/Automated_testing) is a technique where we write code that will test out the application. It is an automation of a manual process that allows to run repetitive tasks without any intervention. It's important to note that testing does not happen at the end of the cycle, rather it is conducted simultaneously with the development.

### Different kinds of tests

There are many [types of tests](https://www.geeksforgeeks.org/types-software-testing/) that can be run in order to ensure our code is working properly, is efficient, clean, and optimized. For the purpose of this course, we will focus on the most common test for React applications.

**Unit tests**

Running tests in the smaller possible units of code from our application. It implies testing individual blocks of code (functions, components) in isolation. Unit tests will assert the expected input to a function that matches the expected output. They are easy to implement in a React project and there are several tools that we can use to run them.

**Integration tests**

Integration tests verify that the combination of multiple building blocks is working correctly. They focus mainly on the flow o data between different modules. Integration tests deal with mocking these 3rd party dependencies and asserting the code interfacing with them behaves as expected.

**End-to-end tests**

End-to-end tests cover the user journey through a specific path in our application. An End2End test plan can be something like "user can add items to cart" or "user can change password". they capture and replay the user's actions and can provide knowledge as to whether a user will have a bug-free experience when performing a specific task.

### Tools

We will need to set up a tool that will run and assert our tests. There are many testing tools out there but one of the most popular testing libraries for Javascript is [Jest](https://jestjs.io/) _"a delightful JavaScript Testing Framework with a focus on simplicity. It allows you to write tests with an approachable, familiar, and feature-rich API that gives you results quickly."_  
Jest is an easy to setup tool that runs fast and will allow you to run tests, easy [mock functions](https://jestjs.io/docs/mock-function-api/), and features like [snapshot testing](https://jestjs.io/docs/snapshot-testing) or a code coverage report.

At the same time, we will also need [React Testing Library](https://testing-library.com/docs/react-testing-library/intro/) which is a library that works with what is in the DOM. It's used with Jest and it's is a set of helpers that lets you test React components without relying on their implementation details and allows you to work with DOM nodes specifically.
Although it doesn’t provide a way to “shallowly” render a component without its children, Jest will let you do this by mocking. Jest is a test runner, React Testing Library is not.

### Writing our first test

If you used create-react-app to create your project, Jest is already installed. If not we suggest you check out the [Jest documentation page](https://jestjs.io/docs/getting-started) and follow the steps for installation according to your project's setup.

Once you are all set up we can create our first test. By convention test files have the same name as the component being tested with the .test extension such as _example.test.js_

In our case, we will create a test for this Greeting.js component:

```javascript
const Greeting = () => {
  return (
    <div>
      <h1>Hello World<h1>
    </div>
  )
}
```

So in our Greeting.test.js file we need to call the test() function which takes two arguments, the first one is the test name which is usually a brief description of the test, and the second one is an anonymous function the expectations to test. The third argument is an optional timeout to wait before aborting, the default is 5 seconds, and we will not alter that.

```javascript
import Greeting from "./Greeting.js";
import { render, screen } from "@testing-library/react";

test("renders Hello World as text", () => {
  render(<Greeting />);
  const helloWorldElem = screen.getByText("Hello World");

  expect(helloWorldElem).toBeInTheDocument();
});
```

In the example above we used the [render](https://testing-library.com/docs/react-testing-library/api#render) function from the React Testing Library in order to render the React element into the DOM. We then used the [screen](https://testing-library.com/docs/queries/about/#screen) object to run a query on a rendered element of the DOM and store that in a variable.  
The [expect](https://jestjs.io/docs/expect#expect) function allows us to test a value. Typically expect will be used with a ["matcher" function](https://jestjs.io/docs/expect#matchers) to assert something about a value. In this case we _expected_ that string to be in the DOM.  
 There are plenty of _matcher functions_ that we can use with the expect method. You can take a look at them by following the above link.

You can run _yarn test_ or _npm test_ to run your test.

#### Grouping tests in a suite

[Describe](https://jestjs.io/docs/api#describename-fn) is a method that creates a block that groups several related tests. This function takes two arguments where the first one is the name of the grouped tests, and the second is an anonymous function that has each individual test.

```javascript
const myBeverage = {
  delicious: true,
  sour: false,
};

describe("my beverage", () => {
  test("is delicious", () => {
    expect(myBeverage.delicious).toBeTruthy();
  });

  test("is not sour", () => {
    expect(myBeverage.sour).toBeFalsy();
  });
});
```

This is helpful for organizing tests into groups, and can even be used to nest tests if they have a defined hierarchy.

### Testing Asynchronous Code

When writing Javascript we usually write code that runs asynchronously, this can cause issues if Jest does not know when the tested code is completed, to move on to another test. To handle these situations Jest has several options:

**1. Return a promise from your test**  
Jest will wait for that promise to resolve and if the promise is rejected, the test will fail.

For example, let's say that fetchData returns a promise that is supposed to resolve to the string 'peanut butter'. We could test it with:

```javascript
test("the data is peanut butter", () => {
  return fetchData().then((data) => {
    expect(data).toBe("peanut butter");
  });
});
```

**2. Use async and await in your tests.**  
Use the async keyword in front of the function passed to test. You can combine async and await with .resolves or .rejects. In the following example, async and await are effectively syntactic sugar for the same logic as the promises example uses.

```javascript
test("the data is peanut butter", async () => {
  await expect(fetchData()).resolves.toBe("peanut butter");
});

test("the fetch fails with an error", async () => {
  await expect(fetchData()).rejects.toMatch("error");
});
```

Jest has other suggested methods to resolve the testing of asynchronous code. You can refer to their [documentation](https://jestjs.io/docs/asynchronous) for more details. The main takeaway here is that when the code you are testing is not synchronous, you must ensure Jest knows about it or it will move into a new test without running the complete code.

### Working with mocks

Many times our components will have http requests that connect to servers and usually, we will want to test how our components behaves according to the outcome of those requests.  
But when we are testing we don't generally want to send http requests to our servers to avoid overloading the network with traffic or even editing database records. We could send requests to testing servers, but we can avoid sending requests entirely by using Jest's mock functions.

[Mock functions](https://jestjs.io/docs/mock-function-api/) allow us to replace and imitate the behavior of dependencies in our code but only when we are running the test. It provides features to capture calls, set return values, or change implementations.  
You can create a mock function with jest.fn() which returns an object of type "mock". If no implementation is given, the mock function will return undefined when invoked.

```javascript
test("async test", async () => {
  const asyncMock = jest.fn().mockResolvedValue(43);

  await asyncMock(); // 43
});
```

In the example above we used **mockFn.mockResolvedValue(value)** which is useful to mock async functions in async tests. In this case, it will return the provided value. There are many mock functions for different scenarios, you can check each case in Jest's [documentation page](https://jestjs.io/docs/mock-function-api/#reference).

This module was only intended to provide you with an introduction to testing in React, and some of the most commonly used tools. If you want to further your knowledge in the matter we suggest to refer to the official documentation linked on each subject and you can also check out this [course by freecodecamp](https://www.freecodecamp.org/news/how-to-test-react-applications/) or plenty other tutorials online.

## Deploying React apps

Up until this moment we have been writing code and testing our application locally. In order to share our application we will need to follow some steps to _deploy_ it.

### Steps

### 1. Test and optimize your code

Before deploying our application we must thoroughly test it and optimize our code for better performance.
One of the ways we can do this is by applying _lazy loading_ in certain components that may make our application slower.

### Lazy loading

As we've seen previously we must import the components we are using for them to work, import connects the different files, and all imported files must be loaded in order for our component to render. This means that before the application is shown to end users, all the imports must be resolved previously to showing something on the screen. In a simple application, this poses no problem but in larger applications, it can cause slow rendering or other performance issues. [Lazy Loading](https://react.dev/reference/react/lazy) can help with this.

**Lazy loading** means that we will only run certain pieces of code when they are needed.
To add lazy loading we need to remove the simple import we've been using. Instead, we will import _lazy_ function from react and call it to declare a lazy-loaded component.

```javascript
import { lazy } from "react";

const MarkdownPreview = lazy(() => import("./MarkdownPreview.js"));
```

The lazy function returns a React component that you can render in your component tree. It takes _load_ as a parameter which is a function that returns a Promise or another Promise-like object with a _then_ method. After the load method is called, React will wait for it to be resolved to render the component.

> _Note:_ Both the returned Promise and the Promise’s resolved value will be cached, so React will not call load more than once. If the Promise rejects, React will throw the rejection reason for the nearest Error Boundary to handle.

React provides a [_Suspense_](https://react.dev/reference/react/Suspense) component that we can use to indicate to the user that the content is loading. We can provide a fallback property to Suspense that we can use until the component is rendered.
You can use it by wrapping the lazy component or any of it's parents with it.

```javascript
<Suspense fallback={<Loading />}>
  <h2>Preview</h2>
  <MarkdownPreview />
</Suspense>
```

Keep in mind you should not declare lazy components _inside_ other components but rather at the top level of your module.

```javascript
import { lazy } from "react";

function Editor() {
  // 🔴 Bad: This will cause all state to be reset on re-renders
  const MarkdownPreview = lazy(() => import("./MarkdownPreview.js"));
  // ...
}
```

```javascript
import { lazy } from "react";

// ✅ Good: Declare lazy components outside of your components
const MarkdownPreview = lazy(() => import("./MarkdownPreview.js"));

function Editor() {
  // ...
}
```

### 2. Build the Application

The code we have written is not the one that will be deployed. Our code is optimized for human readability and sometimes uses features not supported by browsers (like JSX code).  
Instead, we must execute a script _npm run build_ that will create a bundle with a highly optimized and compressed version of our code, and store it in a **build** folder that will be created at the root of our project. And it's the contents of that folder that will be deployed to the server.

### 3. Deploy

Our application is ready to be deployed. It is a static website that only contains HTML, CSS, and Javascript. No code will ran in the server but rather it will be parsed and executed by our visitor's browser.  
The React SPA only requires a static site host.
There are many providers and you can search them online. Typically they will provide you with the steps to upload and deploy your project.  
Here you'll find tutorials for deploying your app in [Firebase](https://create-react-app.dev/docs/deployment/#firebase) or [Github pages](https://create-react-app.dev/docs/deployment/#github-pages), or you can read [this tutorial](https://blog.logrocket.com/8-ways-deploy-react-app-free/) with different options for deploying.
