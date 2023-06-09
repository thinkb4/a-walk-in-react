# [A walk in React](/README.md)

## DAY 4
- [DAY 4](#day-4)
    - [Styling React components](#styling-react-components)
        - [Inline style](#inline-style)
        - [CSS stylesheet](#css-stylesheet)
        - [CSS modules](#css-modules)
        - [Dynamic styles](#dynamic-styles)
    - [useEffect Hook](#useeffect-hook)
        - [What are side effects](#what-are-side-effects)
        - [How to use it](#how-to-use-it)
        - [Dependencies](#dependencies)
        - [When to use it](#when-to-use-it)
        - [The useEffect Cleanup function](#the-useeffect-cleanup-function)


## Styling React components

There are many ways you can style your react components, and whilst React doesn't officially recomend a specific one, we will go over some of the most popular methods used and some of the pros and cons of each of them. These methods can be used simultaneously, but they may interfiere whith one another so it's important to plan ahead and document your styling choices. 

> *Note:* Remember that ultimately we are always writing CSS, the diference lies in the aproaches we take. 

### Inline style
---
React supports inline styling with JSX. You can add the style property in the tag and write the styles inside a Javascript object. The keys should be written using camelCase, and the values if they are pixels you can use numbers, but if you need to specify the mesurement unit such as 'rem' or 'vh' the value mus be a string.  

> *Note:* Remember that JSX uses the curly braces to evaluate Javascript expressions so the syntax will be {{}}.

````javascript
export default function Card() {
  return (
    <div style={{padding: '1rem', border: '1px solid red'}}>
      <h1>Jhon Doe</h1>
      <p>Developer</p>
      <p>Id: 000555444333</p>
    </div>
  );
}
````
 
To improve upon the code readbility you can always extract the JS object. 

````javascript
const cardStyles = {
  padding: '1rem', 
  border: '1px solid red'
}

const infoStyles = {
  fontSize: '12px'
}

export default function Card() {
  return (
    <div style={cardStyles}>
      <h1>Jhon Doe</h1>
      <p>Developer</p>
      <p style={infoStyles}>Id: 000555444333</p>
    </div>
  );
}
````

*Pros*
- Scoped styles
- Easy to apply dynamic styling
- It avoids styles specificity conflicts as the styles are applied to a specific tag in separate components.
- It's easy to get rid of dead code, since it's in smaller files witrh lees lines to go through.

*Cons*
- Must use Javascript syntax which reduces the power of CSS.
- Losing access to media queries, keyframe animations, pseudo selectors.
- No cascading capabilities.

Inline styling is not recommended as the  primary means of styling projects, it's lower preformant than adding classes and it decreases the readibily of the code.  

### CSS stylesheet
---
A more preformant option is adding a stylesheet by createing a new .css file in your proyect directory and importing it into the application.
This approach possibly the most accesible one since all front end developers know how wirte CSS so there is really no new tool to learn, and it requires no dependencies sinse it has native browser support. Still you may run into issues if your proyect is of meduim or large sacel or if you are collaborating with other developers.      
In order to add classes to a component you must use *className*, since class is a Javascript reserved word. 

````javascript
import "./style.css"

export default function Card() {
  return (
    <div className="card">
      <h1>Jhon Doe</h1>
      <p>Developer</p>
      <p>Id: 000555444333</p>
    </div>
  );
}
````
**Pros**  
 - It's easy and fast to implement.
 - Known CSS syntax.
 - You can configure your bundle to use preprocessors like SASS or LESS.

But CSS in it's conception was ment to style a particular HTML document and not diferent components in the same application. the styles get imported into the HTML head tag and that brings the following consecuences:

 **Cons**
- The scope of the styles is global and as the proyect grows in scope the styles interfiere with one another.
- Dead code elimination becomes more difficult as files grow in sizes.
- Even when creating several css files they will all get budled up into the same global scope.
 

In order to avoid this you can use some work arounds like [BEM Naming convention](https://en.bem.info/methodology/naming-convention/#:~:text=%2D%2Dmod%2Dval-,Names%20are%20written%20in%20lowercase%20Latin%20letters.,a%20double%20hyphen%20(%20%2D%2D%20).) whose purpose is to give names meaning so that they are as informative as possible for the developer and at the same time it avoids clas names colliding with each other. 

### Dynamic styles (CSS-in-JS)
---
This styling method can be achived using libraries like: [JSS](https://cssinjs.org/?v=v10.10.0), [Emotion](https://emotion.sh/docs/introduction), [Styled components](https://styled-components.com/) or many others.

At there core this libraries allow us to use CSS in our React components, achiving a component with styling within the same file. They implement it differently so each one has specific documentation.

**Pros**
- Scope styling to specific components.
- Acces to pseudo selectors, media queries and animations.
- Easy to achive dynamic styling based on the state of the application.
- Some of the use CSS syntax.
- Some have pre built patterns wich solve common problems like aplication theming.

**Cons**

- Styles need to be loaded, parsed and exceuted at runtime which makes it slower and increses the time-to-action.
- Styles are defined in the same component file. (Though this is personal preference)
- Need to learn new syntax.

This styling method is agood approach, but it may affect the preformance of your application. Here are some articles on the matter if you wish to learn more about it, but keep in mind it will all depend on many factors, and it's always a matter of finding the right tool for each particular proyect. 

[CSS vs CSS-in-JS Performance](https://medium.com/@pitis.radu/css-vs-css-in-js-performance-bcbdf8e1f6ff)  
[Why We're Breaking Up with CSS-in-JS](https://dev.to/srmagura/why-were-breaking-up-wiht-css-in-js-4g9b)


### CSS modules
---
CSS Module is a CSS file in which all class names and animation names are scoped locally by default tho the component that imports the CSS file.  

The magic of CSS modules happens at build time when the local class names are mapped to custom automatically-generated ones and exported as a JS object literal to use within React.

To apply this approach you will need to create a CSS file and name it 
ComponentName.module.css.
With in the component you will need to import the file and apply the styles in the *className* property as a Javascript object.

````javascript
import styles from '.Button.module.css';

const Button = () => {
  return (
    <button className={styles.error}></button>
  )
}
````
> *Note:* You can use the dot notation to access Javascript object keys. In the example above *styles* is the object the styles are bundeled to.

**Pros:**
- No preformance impact.
- Scoped styles.
- CSS syntax.
- Supports preprocessors.
- Easy to eliminate dead code, since they are all in separate files.

**Cons:**
- We have no way to share constants since CSS and JS are divided.
- CamelCase naming for CSS classes.
- Sharing styles between components and/or with global styles can be difficult.

This is a frecuently used approach with great benefits and little downsides. 

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
- An empty array will caouse the Effect to run once after the  initial render.
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





