# [A walk in React](/README.md)

## DAY 3

- [A walk in React](#a-walk-in-react)
  - [DAY 3](#day-3)
  - [Hooks](#hooks)
    - [What is a Hook?](#what-is-a-hook)
    - [Hook Rules](#hook-rules)
  - [useState Hook](#usestate-hook)
    - [What does the useState call do?](#what-does-the-usestate-call-do)
    - [What do we pass to useState as an argument?](#what-do-we-pass-to-usestate-as-an-argument)
    - [When to use it?](#when-to-use-it)
    - [Import useState](#import-usestate)
    - [Initialize useState](#initialize-usestate)
    - [Read State](#read-state)
    - [Update State](#update-state)
    - [Updating Objects and Arrays in State](#updating-objects-and-arrays-in-state)
  - [useEffect Hook](#useeffect-hook)
    - [What are side effects](#what-are-side-effects)
    - [How to use it](#how-to-use-it)
    - [Dependencies](#dependencies)
    - [When to use it](#when-to-use-it-1)
    - [The useEffect Cleanup function](#the-useeffect-cleanup-function)

## Hooks

### What is a Hook?

In React, "hooks" are a set of functions that allow you to add state and other React features to functional components. Before the introduction of hooks in React 16.8, state management and lifecycle methods were primarily associated with class components. Hooks were introduced to make it possible to use these features in functional components as well, making functional components more powerful and versatile.

You can either use the built-in Hooks or combine them to build your own.

### Hook Rules

- Hooks can only be called at the top level in the body of a function component.
- Hooks can only be called at the top level in the body of a custom Hook.

- Do not call Hooks inside conditions or loops.
- Do not call Hooks after a conditional `return` statement.
- Do not call Hooks in event handlers.
- Do not call Hooks in class components.
- Do not call Hooks inside functions passed to `useMemo`, `useReducer`, or `useEffect`.

## useState Hook

The React useState Hook allows us to track state in a function component.

State generally refers to data or properties that need to be tracking in an application.

### What does the useState call do?

- Declares a “state variable”.
- **useState** is a new way to use the exact same functions that `this.state` gives us in a class.
- Normally, variables "disappear" when the function is exited, but state variables are persisted by React.

### What do we pass to useState as an argument?

The only argument to the useState() Hook is the initial state. What is returned by useState?

Returns a pair of values ​​(array): the current state and a function that updates it.

Summary:

- We declare a state variable called counter and assign it to 0.
- React will remember its current value between re-renders, and will return the most recent value to our function.
- If we want to update the current counter value, we can call setCounter.
- When the user clicks, we call setCounter with a new value. React will then update the Counter component by passing it the new counter value.
- Note the square brackets are Javascript indexes, it's called “array destructuring”.

### When to use it?

The React useState Hook allows us to track state in a function component.

State generally refers to data or properties that need to be tracking in an application.

### Import useState

To use the useState Hook, we first need to import it into our component.

```javascript
import { useState } from "react";
```

> _Note:_ Notice that we are destructuring useState from react as it is a named export.

### Initialize useState

We initialize our state by calling useState in our function component.

useState accepts an initial state and returns two values:

- The current state.
- A function that updates the state.

**Example:**

Initialize state at the top of the function component.

```javascript
import { useState } from "react";

function FavoriteColor() {
  const [color, setColor] = useState("");
}
```

Notice that again, we are destructuring the returned values from useState.
The first value, color, is our current state.
The second value, setColor, is the function that is used to update our state.

> _Note:_ These names are variables that can be named anything you would like.

Lastly, we set the initial state to an empty string.

### Read State

**Example:**

Use the state variable in the rendered component.

```javascript
import { useState } from "react";
import ReactDOM from "react-dom/client";

function FavoriteColor() {
  const [color, setColor] = useState("red");

  return <h1>My favorite color is {color}!</h1>;
}
```

### Update State

To update our state, we use our state updater function.

> _Note:_ We should never directly update state. Ex: color = "red" is not allowed.

**Example:**

Use a button to update the state:

```javascript
import { useState } from "react";

function FavoriteColor() {
  const [color, setColor] = useState("red");

  return (
    <>
      <h1>My favorite color is {color}!</h1>
      <button type="button" onClick={() => setColor("blue")}>
        Blue
      </button>
    </>
  );
}
```

The useState Hook can be used to keep track of primitive values (strings, numbers, booleans, etc.), Arrays and Objects.

We could create multiple state Hooks to track individual values.

**Example:**

Create multiple state Hooks:

```javascript
import { useState } from "react";

function Car() {
  const [brand, setBrand] = useState("Ford");
  const [model, setModel] = useState("Mustang");
  const [year, setYear] = useState("1964");
  const [color, setColor] = useState("red");

  return (
    <>
      <h1>My {brand}</h1>
      <p>
        It is a {color} {model} from {year}.
      </p>
    </>
  );
}
```

Or, we can just use one state and include an object instead!

**Example:**

Create a single Hook that holds an object:

```javascript
import { useState } from "react";

function Car() {
  const [car, setCar] = useState({
    brand: "Ford",
    model: "Mustang",
    year: "1964",
    color: "red",
  });

  return (
    <>
      <h1>My {car.brand}</h1>
      <p>
        It is a {car.color} {car.model} from {car.year}.
      </p>
    </>
  );
}
```

> _Note:_ Since we are now tracking a single object, we need to reference that object and then the property of that object when rendering the component. (Ex: car.brand)

### Updating Objects and Arrays in State

When state is updated, the entire state gets overwritten.

What if we only want to update the color of our car?

If we only called setCar({color: "blue"}), this would remove the brand, model, and year from our state.

We can use the JavaScript spread operator to help us.

**Example:**

Use the JavaScript spread operator to update only the color of the car:

```javascript
import { useState } from "react";

function Car() {
  const [car, setCar] = useState({
    brand: "Ford",
    model: "Mustang",
    year: "1964",
    color: "red",
  });

  const updateColor = () => {
    setCar((previousState) => {
      return { ...previousState, color: "blue" };
    });
  };

  return (
    <>
      <h1>My {car.brand}</h1>
      <p>
        It is a {car.color} {car.model} from {car.year}.
      </p>
      <button type="button" onClick={updateColor}>
        Blue
      </button>
    </>
  );
}
```

Because we need the current value of state, we pass a function into our setCar function. This function receives the previous value.

We then return an object, spreading the previousState and overwriting only the color.

> _Note:_ `previousState` can be named anything you would like.

## useEffect Hook

React works with the DOM to render a UI into the page, we can use JSX to create elements and we've learned how to manage their state with useState. But every time React needs to interact with anything that is outside of it's reach, we can use [the _Effect_ hook](https://react.dev/learn/synchronizing-with-effects). Effect allows to run code after the component has rendered so it can be synchronized with an outside systems.

It's important to remember that as each render has it's own props, state and eventListeners, each render has it's own `Effect`, and because they run after every render, they are a part of the component output, and access it's props and state.  
You can follow [this link](https://overreacted.io/a-complete-guide-to-useeffect/) for a more in depth look at the Effect hook written by Dan Abramov, one of the core contributors to React.

### What are side effects

---

React components should be kept [_pure_](https://react.dev/learn/keeping-components-pure), the render function in a component should calculate a result and generate and output, but not do anything else.  
React is expecting all your components to be written as pure functions, this implies that you mustn't change any objects or variables that existed before rendering, as to not make the component _impure_ and introduce many side effects that could cause bugs and unpredictable behavior.

**Side effects** are anything that is outside of Reacts reach, it could be API/database interaction, using the browser's localStorage or setting up a server connection, in any case they
`cannot happen during render` and so the **Effect** allows you to specify code that will run after a render, and it's side effects will be caused by the rendering and not by a particular event (like a user clicking a button).

### How to use it

---

To declare an Effect in your component you will need to import the hook from React:

```javascript
import { useEffect } from "react";
```

We can then call the Effect hook at the top level of our component by calling the **useEffect()** function which has a required parameter and an optional one.

```javascript
import { useEffect } from "react";

function MyComponent() {
  useEffect(() => {
    // Code here will run after *every* render
  });
  return <div></div>;
}
```

The **first parameter** is _required_ and consists of a callback function that has our side effects code (the code we want to run after the render). It is typically written as an [arrow function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

```javascript
import { useState, useEffect } from "react";

export default function App() {
  const [starWarsData, setStarWarsData] = useState({});

  console.log("Component rendered");

  useEffect(() => {
    fetch("https://swapi.dev/api/people/1")
      .then((res) => res.json())
      .then((data) => setStarWarsData(data));
  });
  return <div>{JSON.stringify(starWarsData, null, 2)}</div>;
}
```

In the example above our useEffect hook is making an API call to retrieve a character from Star Wars. Paste the code into your project and check the console's output.

As you can see we added a [console.log](https://developer.mozilla.org/en-US/docs/Web/API/console/log) that will let us know when the component is being render, and that keeps printing into the console because the component is re rendering on a loop. Try commenting the useEffect function.

This happens because of out missing **second parameter**, the array dependencies.

### Dependencies

---

We mention before that the second parameter is optional, however you will see that it will be necessary in most cases.

The dependencies array determines when the Effect hook will run, if it has no dependencies it will only run once after the initial render. Otherwise it will look out for any changes between one render and the other, and compare the dependencies declared in the array, if they don't match useEffect reruns.

```javascript
import { useState, useEffect } from "react";

export default function App() {
  const [starWarsData, setStarWarsData] = useState({});

  console.log("Component rendered");

  useEffect(() => {
    console.log("Effect ran");

    fetch("https://swapi.dev/api/people/1")
      .then((res) => res.json())
      .then((data) => setStarWarsData(data));
  }, []);

  return <div>{JSON.stringify(starWarsData, null, 2)}</div>;
}
```

Copy this code in your project and check the console. How many times does the component render? How many does the Effect run?

Now, let´s an example with values in the dependencies array:

```javascript
import { useEffect } from "react";

export default function App() {
  const { data } = useSomeCustomHook(); // Imagine that this hook loads some data reacting to an event.

  useEffect(() => {
    console.log("data changed");
  }, [data]);

  return <div>{/* Some JSX... */}</div>;
}
```

In this example, we are reacting to `data`. When data changes, our useEffect will be triggered.

**Summary:**  
The dependencies array should contain every value used by the Effect.

- If there is no array the Effect will ran every time the component renders.
- An empty array will cause the Effect to run once after the initial render.
- Dependencies should include all the values inside the component that are used inside the effect.

> _Note:_ Keep in mind if those values are constantly updated it will cause Effect to constantly rerun.

### When to use it

---

> Effects allows you to “step outside” of React and synchronize your components with some external system.  
> If there is no external system involved (for example, if you want to update a component’s state when some props or state change), you shouldn’t need an Effect. Removing unnecessary Effects will make your code easier to follow, faster to run, and less error-prone.

Remember: The _Effect_ hook is typically used to access **external** systems like APIs, third-party widgets or a network. In the official docs [You might not need an Effect](https://react.dev/learn/you-might-not-need-an-effect) we can see different cases when you may be tempted to use the Effect hook, but the correct approach is explained.  
As general guide you don't need Effect:

- When you want to **transform data for rendering**. You can do so at the top level of your component and the code will re-run whenever the props or state changes.
- When you need to handle user events. by the time the Effect runs, you will no longer know what event the user triggered. User events should be handled by the corresponding event handlers.

### The useEffect Cleanup function

---

When using Effect it's important to _clean up_ (or undo) after the consequences of the side effects our hook causes, preventing unwanted behavior and optimizing our application's performance.

In order to do so, your _Effect_ function can return a function that will clean up any effects caused by the hook running.  
React will then call your cleanup function each time before the Effect runs again, and one final time when the component gets removed, cleaning up effects from the previous render before running the effects again.

**Example:**

```javascript
import { useEffect } from "react";

export default function App() {
  useEffect(
    () => {
      // Some code...

      return () => console.log("Component unmounted"); // This function will be executed when component unmounts.
    },
    [
      /* Dependencies */
    ]
  );

  return <div>{/* Some JSX... */}</div>;
}
```
