# [A walk in React](/README.md)

## DAY 7

- [DAY 7](#day-7)

  - [Events](#events)
    - [Event handlers](#Event-handlers)
  - [Custom React Hooks](#custom-react-hooks)
    - [What are they](#what-are-they)
    - [How to create them](#how-to-create-them)
    - [How to use them](#how-to-use-them)
  - [Debugging React apps](#debugging-react-apps)
    - [Error messages](#error-messages)
    - [React Devtools](#react-devtools)

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
}
```

## Custom React Hooks