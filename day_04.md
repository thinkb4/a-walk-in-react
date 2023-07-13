# [A walk in React](/README.md)

## DAY 4
- [DAY 4](#day-4)
    - [Styling React components](#styling-react-components)
        - [Inline style](#inline-style)
        - [CSS stylesheet](#css-stylesheet)
        - [CSS modules](#css-modules)
        - [Dynamic styles](#dynamic-styles)
    - [useRef Hook](#useref-hook)
    

## Styling React components

There are many ways you can style your react components, and whilst React doesn't officially recommend a specific one, we will go over some of the most popular methods used and some of the pros and cons of each of them. These methods can be used simultaneously, but they may interfiere whith one another so it's important to plan ahead and document your styling choices. 

> *Note:* Remember that ultimately we are always writing CSS, the difference lies in the aproaches we take. 

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
A more preformant option is adding a stylesheet by creating a new .css file in your proyect directory and importing it into the application.
This approach possibly the most accesible one since all front end developers know how to write CSS so there is really no new tool to learn, and it requires no dependencies since it has native browser support. Still you may run into issues if your proyect is of medium or large sacel or if you are collaborating with other developers.      
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
