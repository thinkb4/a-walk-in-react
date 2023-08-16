# [A walk in React](/README.md)

## DAY 3

- [DAY 3](#day-3)
  - [Class Based Components (legacy)](#class-based-components)
    - [Class based vs Functional components](#class-based-vs-functional-components)
  - [Hooks](#hooks)
    - [State](#state)
    - [useState Hook](#useState-hook)
    - [How to update State](#how-to-update)
    - [Controlled or uncontrolled components](#controlled-or-uncontrolled-components)
  - [Effects](#effects)
    - [useEffect Hook](#useEffect-hook)
    - [What are side effects](#what-are-side-effects)
    - [When to use it](#when-to-use-it)
    - [Dependencies](#dependencies)
    - [The useEffect Cleanup function](#the-useEffect-cleanup)


- [useEffect Hook](#useeffect-hook)
        - [What are side effects](#what-are-side-effects)
        - [How to use it](#how-to-use-it)
        - [Dependencies](#dependencies)
        - [When to use it](#when-to-use-it)
        - [The useEffect Cleanup function](#the-useeffect-cleanup-function)

## useEffect Hook

React works with the DOM to render a UI into the page, we can use JSX to create elements and we've learned how to manage their state with useState. But every time React needs to interact with anything that is outside of it's reach, we can use [the *Effect* hook](https://react.dev/learn/synchronizing-with-effects). Effect allows to run code after the component has rendered so it can be sincronized with an outside systems.  


It's important to remember that as each render has it's own props, state and eventListeners, each render has it's own *Effect*, and because they run after every render, they are a part of the component output, and access it's props and state.  
You can follow [this link](https://overreacted.io/a-complete-guide-to-useeffect/) for a more in depth look at the Effect hook written by Dan Abramov, one of the core contributors to React.

### What are side effects
---
 
React components should be kept [*pure*](https://react.dev/learn/keeping-components-pure), the render function in a component should calculate a result and generate and output, but not do anything else.  
React is expecting all your components to be written as pure functions, this implies that you mustn't change any objects or variables that existed before rendering, as to not make the component *impure* and introduce many side effects that could cause bugs and unpredicatble behaviour.   


**Side effects** are anything that is outside of Reacts reach, it could be API/database interaction, using the browser's localStorage or setting up a server connection, in any case they 
*cannot happen during* render and so the **Effect** allows you to specify code that will run after a render, and it's side effects will be caused by the rendering and not by a particular event (like a user clicking a button).

### How to use it
---
To declare an Effect in your component you will need to import the hook from React:

````javascript
import { useEffect } from 'react';
````

We can then call the Effect hook at the top level of our component by calling the **useEffect()** function which has a required parameter and an optional one. 

````javascript
import { useEffect } from 'react';

function MyComponent() {
  
  useEffect(() => {
    // Code here will run after *every* render
  });
  return <div></div>;
}
````
The **first parameter** is *required* and consists of a callback function that has our side effects code (the code we want to run after the render). It is typically written as an [arrow function](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions).

````javascript
import { useState, useEffect } from 'react';

export default function App() {
  const [starWarsData, setStarWarsData] = useState({})

  console.log("Component rendered")

  useEffect(() => {
    fetch("https://swapi.dev/api/people/1")
            .then(res => res.json())
            .then(data => setStarWarsData(data))
  });
  return <div>{JSON.stringify(starWarsData, null, 2)}</div>;
}
````

In the example above our useEffect hook is making an API call to retrieve a character from Star Wars. Paste the code into your proyect and check the console's output.  


As you can see on line 6 we added a [*console.log*](https://developer.mozilla.org/en-US/docs/Web/API/console/log) that will let us know when the component is being render, and that keeps printing into the console because the component is re rendering on a loop. Try commenting the useEffect function.

````javascript
import { useState, useEffect } from 'react';

export default function App() {
  const [starWarsData, setStarWarsData] = useState({})

  console.log("Component rendered")

  useEffect(() => {
    fetch("https://swapi.dev/api/people/1")
            .then(res => res.json())
            .then(data => setStarWarsData(data))
  });
  return <div>{JSON.stringify(starWarsData, null, 2)}</div>;
}
````

This happens because of out missing **second parameter**, the array dependencies.

### Dependencies
---
We mention before that the second parameter is optional, however you will see that it will be necesary in most cases.   
The dependencies array determines when the Effect hook will run, if it has no dependencies it will only run once after the initial render. Otherwise it will look out for any changes between one render and the other, and compare the dependencies declared in the array, if they don't match useEffect reruns.


````javascript
import { useState, useEffect } from 'react';

export default function App() {
  const [starWarsData, setStarWarsData] = useState({})

  console.log("Component rendered")

  useEffect(() => {
    console.log("Effect ran")
    fetch("https://swapi.dev/api/people/1")
            .then(res => res.json())
            .then(data => setStarWarsData(data))
  }, []);
  return <div>{JSON.stringify(starWarsData, null, 2)}</div>;
}
````

Copy this code in your project and check the console. How many times does the component render? How many does the Effect run?

**Summary:**   
The dependencies array should contain every value used by the Effect.
- If there is no array the Effect will ran every time the component renders.
- An empty array will cause the Effect to run once after the  initial render.
- Dependencies should include all the values inside the component that are used inside the effect.

> *Note:* Keep in mind if those values are constantly updated it will cause Effect to constantly rerun.

### When to use it
---

> Effects allows you to “step outside” of React and synchronize your components with some external system.  
If there is no external system involved (for example, if you want to update a component’s state when some props or state change), you shouldn’t need an Effect. Removing unnecessary Effects will make your code easier to follow, faster to run, and less error-prone.

Remember: The *Effect* hook is typically used to access **external** systems like APIs, third-party widgets or a network. In the official docs [You might not need an Effect](https://react.dev/learn/you-might-not-need-an-effect) we can see different cases when you may be tempted to use the Effect hook, but the correct approach is explained.   
As general guide you don't need Effect:
- When you want to **transform data for rendering**. You can do so at the top level of your component and the code will re-run whenever the props or state changes.
- When you need to handle user events. by the time the Effect runs, you will no longer know what event the user triggered. User events should be handeled by the corresponding event handelers.   

### The useEffect Cleanup function
---
When using Effect it's important to *clean up* (or undo) after the consecuences of the side effects our hook causes, preventing unwanted behaviour and optimizing our application's prefomance.  

In order to do so, your *Effect* function can return a function that will clean up any effects caused by the hook running.  
React will then call your cleanup function each time before the Effect runs again, and one final time when the component gets removed, cleanning up effects from the previous render before running the effects again.