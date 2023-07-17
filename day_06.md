# [A walk in React](/README.md)

## DAY 6

- [DAY 6](#day-6)

  - [useReducer](#usereducer)
  - [Performance Hooks](#performance-hooks)
    - [useMemo](#usememo)
    - [useCallback](#usecallback)
  - [Forms in React](#forms-in-react)

## useReducer

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

// This is because state behaves like a snapshot The state update requests another render with the new state value, but does not affect the JavaScript variable in its statehandler of events already running.
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
> - calculateValue: The function that calculates the value you want to memoize. It must be pure, it must not accept arguments, and it must return a value of any type. React will call your function during initial rendering. On subsequent renders, React will return the same value again if the values dependencias​​haven't changed since the last render. Otherwise, it will call **calculateValue**, return its result, and store it in case it can be reused later.
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
