# [A walk in React](/README.md)

## DAY 2

- [DAY 2](#day-2)
    - [Components](#components)
        - [My first component](#my-first-component)
        - [Reusable components](#reusable-components)
        - [JSX](#jsx)
    - [Props](#props)
    - Conditional rendering
    - Events
    - Hooks

> *Note:* As we move further in the course, we encourage you to copy and paste the code examples we provide in the application we created in the first lessson. Be sure to run the application so you can see you first React code in action! And feel free to make your own modifications and exprement with the results.   

## Components  

React components are the fundation on which react applicatios are built. They are "reusable UI elements" that allow you to combine the markup, CSS and Javascript in one single building block that can be ordered, nested and reused in order to create entire applications.

> A React component is a JavaScript function that you can sprinkle with markup.   
 Source: [React Oficial Documentation](https://react.dev/learn/your-first-component) 

### My first component

Let's take a look at a simple React component


```javascript
export default function MyFirstComponent() {
  return <h1>My First Component</h1>
}
```
As you can see the main function has a prefix if export default which is a standard Javascript syntax that will allow oyu to improt the file from other files.

> *Note:* React components are regular JavaScript functions, but their names must start with a capital letter or they won’t work! this allows React to undestard if they are Components or HTML tags.

Te components returns an h1 tag written like HTML but it is actually JSX, a syntax that allows to embed markup inside JavaScript (More on JSX later)

If your component returns more than one line of code you must wrap it in parenthesis, like in the following example:

```javascript
export default function MyFirstComponent() {
  return (
    <>
        <h1>My First Component</h1>
        <p>Hello world</p>
    </>
  )
}
```

### Reusable components

Now that we have created our first React component, we can nest it inside others as many times as we want. 

```javascript
function MyFirstComponent() {
    return <p>Hello world</p>
}

export default function MyComponens() {
    return (
        <section>
            <h1>Awesome components</h1>
            <MyFirstComponent />
            <MyFirstComponent />
            <MyFirstComponent />
        </section>
    );
}
```

In the code above we could say MyComponents is the "parent" and MyFirstComponent is the "child" component. You can keep multiple components in the same file, but you must never nest their definitions.   
As you files grow larger and your components more complex, you can move your components to separate files and import them where you will need them. 

```javascript
// 🔴 Never define a component inside another component!  
export default function MyComponents() {  
  function MyFirstComponent() {  
    // ...  
  }  
  // ...  
}
```

The snippet above is very slow and causes bugs. Instead, define every component at the top level:

```javascript
 //✅ Declare components at the top level  
 export default function Gallery() {  
  // ...  
 }  
 function Profile() {  
  // ...  
 }
```

### Import and export components

In order to import the components you want to use into other components you can follow this syntax:

```javascript
 import MyFirstComponent from './MyFirstComponent.js';
 
 export default function App() {  
  return (  
    <MyFirstComponent />  
  );  
}
```

You may see some code that leaves off the .js file extension in the import, and that will work with React, though the former is closer to how native ES Modules work.

> *Note:* There are two primary ways to export values with JavaScript: default exports and named exports. So far, our examples have only used default exports. But you can use one or both of them in the same file. A file can have no more than one default export, but it can have as many named exports as you like. This will affect how you import the file. Checkout this [MDN doc](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#description) for more info.


### JSX

The official React docs define JSX as *"a syntax extension for JavaScript that lets you write HTML-like markup inside a JavaScript file."*

You can still write vanilla Javascript to create a div or to add a title to your component, but what JSX allows you to do is to continue using the HTML markup we know, adding the hability to display dynamic content. That said JSX is not HTML, but rather it allows you to write HTML into a Javascript file, with a few more rules.

1. **Return a single root element:** All elements from a component, must be wraped in a single parent tag. You can use an empty tag to avoid using unnesesary tags in your markup. 

```javascript
<>
  <h1>My first Component</h1>
  <p>Hello world</p>
</>
```

> *Note:* The empty tag is called [Fragment](https://react.dev/reference/react/Fragment) and it allows you to group elements without affecting the HTML markup.

2. **Close all the tags:** Even self-closing tags like < img > mas have a closing forward slash < img />.

3. **camelCase most things:** Because of Javascript limitations in variable names or reserved words like 'class', attribute names must be in camelCase and 'class' should be replaced with 'className'. 

> *Note:* Only aria-* and data-* attributes are written with dashes, as in HTML.

But the magic of JSX lies in the fact that even within your markup you can write JavaScript code. you may need to add logic or reference a dynamic property, and for that purpose you can use curly braces and open a JavScript window in your code.

```javascript
export default function TodoList() {
  const name = 'Gregorio Y. Zara';
  return (
    <h1>{name}'s To Do List</h1>
  );
}
```

> *Note:* Any JavaScript expression will work between curly braces, including function calls 

You can only use curly braces in two ways inside JSX:

1. **As text** inside a JSX tag: 
```javascript
<h1> {name}'s To Do List </h1>
```
2. **As attributes** immediately following the = sign.   
```javascript
src={name}
``` 

> *Note:* If you wiss to pass in a JavaScript object you can use double cursly braces, which is no special syntax, it’s just a JavaScript object tucked inside JSX curly braces. 


### Props