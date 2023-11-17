# [A walk in React](/README.md)

## DAY 6

- [A walk in React](#a-walk-in-react)
  - [DAY 6](#day-6)
  - [Other React built-in hooks](#other-react-built-in-hooks)
    - [useRef Hook](#useref-hook)
    - [useReducer](#usereducer)
  - [Performance Hooks](#performance-hooks)
    - [useMemo](#usememo)
    - [useCallback](#usecallback)
  - [React Custom Hooks](#react-custom-hooks)
    - [What are they](#what-are-they)
    - [How to create them](#how-to-create-them)
    - [How to use them](#how-to-use-them)

## Other React built-in hooks

### useRef Hook

The useRef hook is a built-in hook in React that allows you to create a mutable reference that persists across re-renders of a functional component. It provides a way to access and manipulate DOM elements or any other mutable value without triggering a re-render.

When you use the useRef hook, it returns a mutable object with a **.current** property. This **.current** property can be used to store any value and is not affected by component re-renders. It maintains the same value between renders, unlike normal variables within the component function body.

```javascript
const ref = useRef(initialValue);
```

**initialValue**: The value you want the ref object’s current property to be initially. It can be a value of any type. This argument is ignored after the initial render.

```javascript
const ref = useRef(initialValue);

function doSomething() {
  console.log(ref.current);
  //...
}
```

useRef returns an object with a single property: current.

**current**: Initially, it’s set to the initialValue you have passed. You can later set it to something else. If you pass the ref object to React as a ref attribute to a JSX node, React will set its current property.

It’s particularly common to use a ref to manipulate the DOM. React has built-in support for this.

```javascript
import { useRef } from "react";

function MyComponent() {
  const inputRef = useRef(null);
  // ...

  return <input ref={inputRef} />;
}
```

After React creates the DOM node and puts it on the screen, React will set the current property of your ref object to that DOM node. Now you can access the **input’s** DOM node and call methods.

For example:

```javascript
import { useRef } from "react";

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleClick}>Focus the input</button>
    </>
  );
}
```

All the code examples and explanations are from https://react.dev/reference/react/useRef

> **Important!**
>
> Do not write or read **ref.current** during rendering. React expects that the body of your component behaves like a pure function.
>
> ```javascript
> function MyComponent() {
>   // ...
>   // 🚩 Don't write a ref during rendering
>   myRef.current = 123;
>   // ...
>   // 🚩 Don't read a ref during rendering
>   return <h1>{myOtherRef.current}</h1>;
> }
> ```
>
> You can read or write refs from event handlers or effects instead.
>
> ```javascript
> function MyComponent() {
>   // ...
>   useEffect(() => {
>     // ✅ You can read or write refs in effects
>     myRef.current = 123;
>   });
>   // ...
>   function handleClick() {
>     // ✅ You can read or write refs in event handlers
>     doSomething(myOtherRef.current);
>   }
>   // ...
> }
> ```
>
> Source: https://react.dev/reference/react/useRef

### useReducer

**useReducer** hook is a powerful tool that allows you to manage complex state logic in your components. It is an alternative to the more commonly used **useState hook**, and it provides a way to update state based on previous state and an action.

The useReducer hook follows the concept of the reducer pattern, which is widely used in functional programming. It takes in two parameters: a **reducer function** and an **initial state**. The reducer function specifies how the state should be updated based on the action dispatched.

Here's an example to illustrate how useReducer works:

```javascript
import React, { useReducer } from "react";

// Reducer function
function reducer(state, action) {
  switch (action.type) {
    case "INCREMENT":
      return {
        ...state,
        count: state.count + 1,
      };
    case "DECREMENT":
      return {
        ...state,
        count: state.count - 1,
      };
    default:
      throw new Error("Invalid action");
  }
}

// Component using useReducer
export default function Counter() {
  const [state, dispatch] = useReducer(reducer, {
    name: "John",
    count: 0,
  });

  const increment = () => {
    dispatch({ type: "INCREMENT" });
  };

  const decrement = () => {
    dispatch({ type: "DECREMENT" });
  };

  return (
    <div>
      <h3>`Hello, ${state.name}!`</h3>

      <p>Count: {state.count}</p>

      <button onClick={increment}>Increment</button>
      <button onClick={decrement}>Decrement</button>
    </div>
  );
}
```

In this example, we define a reducer function that takes in the **current state** and an **action**. The reducer function uses a **switch** statement to determine how the state should be updated based on the action type. In this case, we handle two actions: 'INCREMENT' and 'DECREMENT'. The reducer returns a new state object that reflects the updated count.

In the Counter component, we initialize the state using useReducer. The initial state is an object with a single property called **count** set to **0**. The useReducer hook returns an array with two elements: the current state and a dispatch function. The dispatch function is used to dispatch actions to the reducer.

The **dispatch** function invokes the reducer function, passing the current state and the action. The reducer function uses a switch statement to determine how the state should be updated based on the action type. In this case, it increments or decrements the count property in the state object. By calling dispatch with different action types and payloads, you can trigger different state updates in your component. It's important to note that the dispatch function is synchronous and does not immediately update the state. Instead, React re-renders the component with the new state after the dispatch operation is complete. Using the dispatch function with useReducer provides a predictable way to update state in your React components, especially when dealing with more complex state transitions and actions.

We define two event handlers, increment and decrement, which dispatch the corresponding actions to the reducer when the buttons are clicked.

Finally, we render the current count value from the state and attach the event handlers to the buttons.

By using useReducer, we can manage more complex state updates that involve multiple values or complex state transitions. It provides a structured way to handle state changes and can make your code more maintainable and easier to reason about.

> ### useReducer Params
>
> - reducer: The reducing function that should return the initial state. It must be pure, it must take the state and the action as arguments, and it must return the following state. The state and action can be of any type.
> - initialArg: The value from which the initial state is calculated. It can be a value of any type. How the initial state is calculated depends on the following argument init.
> - optional init : The initializer function that specifies how the initial state is calculated. If not specified, the initial state is set to initialArg. Otherwise, the initial state is the result of calling init(initialArg).
>
> **useReducer returns an array with exactly two values:**
>
> - The current **state**. During the first render, it is set to init(initialArg)o initialArg(if there is no init).
> - The function **dispatch** that allows you to update the state to a different value and trigger a new render.
>
> ### Warnings
>
> **useReducer** is a Hook, so you can only call it at the top level of your component or on your own Hooks. You can't call it inside loops or conditions. If you need to, check out a new component and move state to it.
>
> In Strict Mode, React will call your reducer and initializer twice to help you find accidental impurities. This is development-only behavior and does not affect production. If your reducer and initializer are pure (as they should be), this shouldn't affect your logic. The result of one of the calls is ignored.
>
> Source: https://es.react.dev/reference/react/useReducer

**Attention!**

The status is read-only. Do not modify any objects or arrays of the state:

```javascript
function reducer(state, action) {
  switch (action.type) {
    case "incremented_age": {
      // 🚩 Don't mutate an object in state like this:
      state.age = state.age + 1;
      return state;
    }
  }
}
```

Instead, always return new objects from your reducer:

```javascript
function reducer(state, action) {
  switch (action.type) {
    case "incremented_age": {
      // ✅ Instead, return a new object
      return {
        ...state,
        age: state.age + 1,
      };
    }
  }
}
```

Remember to always consider the asynchronous lifecycle of state updates in React to avoid incorrect assumptions about the updated state

```javascript
function handleClick() {
  console.log(state.age); // 42

  dispatch({ type: "incremented_age" }); // Request a re-render with 43
  console.log(state.age); // Still 42!

  setTimeout(() => {
    console.log(state.age); // Also 42!
  }, 5000);
}

// This is because state behaves like a snapshot The state update requests another render with the new state value, but does not affect the JavaScript variable in its state handler of events already running.
```

Furthermore, useReducer provides an alternative to the useState hook and can be particularly valuable when you need to manage more advanced state management scenarios, such as maintaining a history of state changes or handling asynchronous updates. By understanding and effectively utilizing the useReducer hook, you can enhance your React applications with a more structured and scalable approach to managing state complexity.

In summary, the useReducer hook empowers you to handle intricate state management scenarios in React, promoting better code organization, maintainability, and scalability.

## Performance Hooks

A common way to optimize rendering performance is to avoid unnecessary work. For example, you can tell React to reuse a cached calculation or skip a rerender if the data hasn't changed since the previous render.

### useMemo

**useMemo** hook is used to optimize the performance of a component by memoizing the result of a function call. It allows you to cache the computed value of an expensive function and only recalculate it when its dependencies change.

The useMemo hook takes two parameters: a **function** and an **array of dependencies**. The function represents the expensive computation that you want to memoize, and the dependencies are the values that the function depends on.

Let's see an example:

```javascript
import React, { useMemo } from "react";

function MyComponent({ list }) {
  const expensiveResult = useMemo(() => {
    // Expensive computation here
    console.log("Calculating expensive result...");
    return list.map((item) => item * 2);
  }, [list]);

  return (
    <div>
      <p>Expensive Result: {expensiveResult.join(", ")}</p>
    </div>
  );
}
```

In this example, we have a component **'MyComponent'** that receives a **'list'** prop. We want to perform an expensive computation, such as mapping each item of the list and multiplying it by 2. However, we don't want this computation to occur on every render, especially if the **'list'** prop hasn't changed.

By wrapping the computation with **useMemo**, we memoize the result and ensure that it is only recalculated when the **'list'** prop changes. The dependencies array **'[list]'** tells React to recompute the result whenever the value of list changes.

The first time **'MyComponent'** renders, the expensive computation will run and the result will be stored. On subsequent renders, if the **'list'** prop remains the same, the cached result will be returned without re-computing the expensive function. This optimization can significantly improve the performance of your component.

It's important to note that the **useMemo** hook should be used when the computation is truly expensive and the result is needed in the rendering of the component. If the computation is simple or the result isn't used for rendering, using useMemo may introduce unnecessary complexity.

Additionally, keep in mind that useMemo returns the **cached value, not a function**. If you need to memoize a function itself, you can use the [useCallback](#usecallback) hook instead, which is similar to useMemo but specifically designed for memoizing functions.

By utilizing the useMemo hook, you can optimize the performance of your React components by selectively memoizing expensive computations, ensuring they are only recalculated when their dependencies change.

> ### useMemo parameters
>
> - calculateValue: The function that calculates the value you want to memoize. It must be pure, it must not accept arguments, and it must return a value of any type. React will call your function during initial rendering. On subsequent renders, React will return the same value again if the values dependencies ​​haven't changed since the last render. Otherwise, it will call **calculateValue**, return its result, and store it in case it can be reused later.
>
> - dependencies: The list of all reactive values ​​referenced within the code **calculateValue**. Reactive values ​​include props, state, and all variables and functions declared directly within the body of your component.
>
> Caching values ​​like this is also known as [memoization](https://es.wikipedia.org/wiki/Memoizaci%C3%B3n) , which is why the Hook is called **useMemo**.
>
> ### Warnings:
>
> - **useMemo** is a Hook, so it can only be called at the top level of the component or its own Hooks. You can't call it inside loops or conditions. If you need to, check out a new component and move state to it.
>
> - In strict mode, React will call your calculation function twice to help you find accidental impurities. This behavior occurs only in development and does not affect production. If your compute function is pure (as it should be), this shouldn't affect the logic of your component. The result of one of the calls will be ignored.
>
> - React will not discard the cached value unless there is a specific reason to do so. For example, in development, React will flush the cache when you edit your component file. In both development and production, React will flush the cache if your component is suspended during initial assembly.
>
> Source: https://es.react.dev/reference/react/useMemo

On initial rendering, **useMemo** returns the result of calling with **calculateValue** no arguments.

During subsequent renders, it will either return a value already stored from the last render (if the dependencies haven't changed), or it will call **calculateValue** again and return whatever result it **calculateValue** returned.

### useCallback

**useCallback** hook is used to memoize a function so that it doesn't get recreated on every render, especially when it's passed as a prop to child components. Similar to the [useMemo](#usememo) hook, **useCallback** allows you to optimize performance by memoizing a function, ensuring its reference remains stable unless its dependencies change.

The useCallback hook takes two parameters: a **function** and an **array of dependencies**. The function represents the callback function that you want to memoize, and the dependencies are the values that the function depends on.

Let's see an example from [React official documentation](https://react.dev/reference/react/useCallback):

```javascript
import { memo } from "react";

const ShippingForm = memo(() => ShippingForm({ onSubmit }) {
  // ...
});

function ProductPage({ productId, referrer, theme }) {
  // Every time the theme changes, this will be a different function...
  function handleSubmit(orderDetails) {
    post("/product/" + productId + "/buy", {
      referrer,
      orderDetails,
    });
  }

  return (
    <div className={theme}>
      {/* ... so ShippingForm's props will never be the same, and it will re-render every time */}
      <ShippingForm onSubmit={handleSubmit} />
    </div>
  );
}
```

> In JavaScript, a function `() {} or () => {}` always creates a different function, similar to how the `{}` object literal always creates a new object. Normally, this wouldn’t be a problem, but it means that your component props will never be the same, and your memo optimization won’t work. This is where useCallback comes in handy:

```javascript
function ProductPage({ productId, referrer, theme }) {
  // Tell React to cache your function between re-renders...
  const handleSubmit = useCallback(
    (orderDetails) => {
      post("/product/" + productId + "/buy", {
        referrer,
        orderDetails,
      });
    },
    [productId, referrer]
  ); // ...so as long as these dependencies don't change...

  return (
    <div className={theme}>
      {/* ...ShippingForm will receive the same props and can skip re-rendering */}
      <ShippingForm onSubmit={handleSubmit} />
    </div>
  );
}
```

> By wrapping handleSubmit in useCallback, you ensure that it’s the same function between the re-renders (until dependencies change). You don’t have to wrap a function in useCallback unless you do it for some specific reason. In this example, the reason is that you pass it to a component wrapped in memo, and this lets it skip re-rendering. There are other reasons you might need useCallback which are described further on this page.

This memoization prevents unnecessary re-creation of the callback function on every render, which can be particularly beneficial when passing the function as a prop to child components. If the dependencies in the array change, the function will be re-created with the updated references.

By using **useCallback**, you can optimize the performance of your components by ensuring that functions are only recreated when necessary, preserving referential equality and preventing unnecessary re-renders of child components.

**It's important to note that useCallback should be used when you need to optimize the performance of your components and ensure that the function reference remains stable. If the function doesn't have any dependencies or its reference doesn't affect rendering behavior, using useCallback may not be necessary.**

Also, keep in mind that **useCallback** returns a memoized version of the function, not the function itself. If you need to memoize a value other than a function, you can use the [useMemo](#usememo) hook.

By utilizing the **useCallback** hook effectively, you can optimize the performance of your React components by memoizing callback functions, ensuring their stability and minimizing unnecessary re-renders.

> ### useCallback parameters
>
> - fn: The function value that you want to cache. It can take any arguments and return any values. React will return (not call!) your function back to you during the initial render. On next renders, React will give you the same function again if the dependencies have not changed since the last render. Otherwise, it will give you the function that you have passed during the current render, and store it in case it can be reused later. React will not call your function. The function is returned to you so you can decide when and whether to call it.
>
> - dependencies: The list of all reactive values referenced inside of the **fn** code. Reactive values include props, state, and all the variables and functions declared directly inside your component body.
>
> You should only rely on **useCallback** as a performance optimization.
>
> ### Warnings:
>
> - **useCallback** is a Hook, so it can only be called at the top level of the component or its own Hooks. You can't call it inside loops or conditions. If you need to, check out a new component and move state to it.
>
> - React will not throw away the cached function unless there is a specific reason to do that. For example, in development, React throws away the cache when you edit the file of your component. Both in development and in production, React will throw away the cache if your component suspends during the initial mount.
>
> Source: https://react.dev/reference/react/useCallback

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
