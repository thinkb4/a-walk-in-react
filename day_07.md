# [A walk in React](/README.md)

## DAY 7

- [DAY 7](#day-7)

  - [Events](#events)
    - [Event handlers](#Event-handlers)
  - [React Custom Hooks](#react-custom-hooks)
    - [What are they](#what-are-they)
    - [How to create them](#how-to-create-them)
    - [How to use them](#how-to-use-them)
  - [Debugging React apps](#debugging-react-apps)
    - [Error messages](#error-messages)
    - [React DevTools](#react-devtools)

## Events

Events in React are a fundamental concept that allows you to make your web applications interactive by responding to user actions like clicks, keyboard input, and more. React events are similar to standard DOM events in JavaScript but are managed and abstracted by React to provide a consistent and efficient way to handle user interactions within your components. By understanding how to handle events and use event objects, you can create dynamic and user-friendly interfaces.

### Event handlers

To handle events in React, you attach event handlers to your JSX elements. These event handlers are functions that get executed when a specific event occurs, such as a button click or a keyboard key press.

Here's a basic example of how you can add an event handler to a button element in JSX:

```javascript
import React from "react";

export default function MyComponent() {
  const handleClick = () => {
    alert("Button clicked!");
  };

  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}
```

In this example, when the "Click Me" button is clicked, the handleClick function is executed, showing an alert message.

React event handlers receive an event object as an argument. This object contains information about the event, such as the target element and event type. You can access this object in your event handler function:

```javascript
import React from "react";

export default function MyComponent() {
  const handleClick = (event) => {
    alert("Button clicked!");
    console.log(event.target); // Access the target element
  };

  return (
    <div>
      <button onClick={handleClick}>Click Me</button>
    </div>
  );
}
```

React supports various event types, including `onClick`, `onChange`, `onSubmit`, `onKeyDown`, and many more. The event you choose depends on the type of user interaction you want to capture. In some cases, you might want to prevent the default behavior of an event, like stopping a form submission. You can achieve this by calling `event.preventDefault()` in your event handler.

```javascript
const handleFormSubmit = (event) => {
  event.preventDefault();
  // ...
};
```

## React Custom Hooks

### What are they

> React comes with several built-in Hooks like `useState` , `useContext`, and `useEffect`. Sometimes, you’ll wish that there was a Hook for some more specific purpose: for
> example, to fetch data, to keep track of whether the user is online, or to connect to a chat room. You might not find these Hooks in React, but you can create your own
> Hooks for your application’s needs.
>
> Source: https://react.dev/learn/reusing-logic-with-custom-hooks

Custom Hooks are a powerful and essential concept in React that allows to encapsulate and reuse stateful logic across multiple components. They are a way to abstract and share logic between functional components, making your code more modular, maintainable, and reusable.

### How to create them

To create a custom hook, you follow these steps:

1. **Create a new file:** Start by creating a new JavaScript file for your custom hook. It's a good practice to name your hook with the word "use" followed by a descriptive name to make it clear that it's a custom hook.

2. **Define the Custom Hook:** Create a function that encapsulates the stateful logic you want to reuse. This function should follow the React Hook naming convention by starting with "use"

```javascript
function useForm(initialValues) {
  // Define your state and logic here
}
```

3. **Implement State and Logic**: Inside your custom hook, you can use built-in React hooks like `useState`, `useEffect`, or other custom hooks as needed to manage state and logic. For instance, if you're creating a form handling hook, you might use useState to manage form fields and their values.

```javascript
import { useState } from "react";

function useForm(initialValues) {
  const [values, setValues] = useState(initialValues);

  // Define form-related logic here

  return {
    values,
    handleChange,
    handleSubmit,
    // Add any other functions or data you want to expose
  };
}
```

4. **Return Exposed Functionality**: Return the state and functions you want to expose to components that use your custom hook. In this example, we return the form values and two functions, handleChange and handleSubmit, which components can use.

### How to use them

In your React components, you can now import and use the custom hook you created. Import it just like you would import any other function.

```javascript
import useForm from './useForm';

export default function MyComponent() {
  const { values, handleChange, handleSubmit } = useForm({
    // Initial form values
    name: '',
    email: '',
  });

  // Use the exposed values and functions
  // ...

  return (
    // Your component JSX
  );
}
```

> _Note:_ Custom Hooks let you share stateful logic but not state itself. Each call to a Hook is completely independent from every other call to the same Hook.
>
> Source: https://react.dev/learn/reusing-logic-with-custom-hooks

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
