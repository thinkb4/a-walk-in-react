## useRef Hook

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
