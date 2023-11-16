# [A walk in React](/README.md)

## DAY 2

- [A walk in React](#a-walk-in-react)
  - [DAY 2](#day-2)
  - [Components](#components)
    - [My first component](#my-first-component)
    - [Reusable components](#reusable-components)
    - [Import and export components](#import-and-export-components)
  - [JSX](#jsx)
  - [Props](#props)
    - [What are props?](#what-are-props)
    - [Props default value](#props-default-value)
    - [Props as children](#props-as-children)
  - [Class based components](#class-based-components)
    - [Class based vs Functional components](#class-based-vs-functional-components)

> _Note:_ As we move further in the course, we encourage you to copy and paste the code examples we provide in the application we created in the first lesson. Be sure to run the application so you can see you first React code in action! And feel free to make your own modifications and experiment with the results.

## Components

React components are the foundation on which react applications are built. They are "reusable UI elements" that allow you to combine the markup, CSS and Javascript in one single building block that can be ordered, nested and reused in order to create entire applications.

> A React component is a JavaScript function that you can sprinkle with markup.  
> Source: [React Official Documentation](https://react.dev/learn/your-first-component)

### My first component

Let's take a look at a simple React component

```javascript
export default function MyFirstComponent() {
  return <h1>My First Component</h1>;
}
```

As you can see the main function has a prefix if export default which is a standard Javascript syntax that will allow you to import the file from other files.

> _Note:_ React components are regular JavaScript functions, but their names must start with a capital letter or they won’t work! this allows React to understand if they are Components or HTML tags.

Te components returns an h1 tag written like HTML but it is actually JSX, a syntax that allows to embed markup inside JavaScript (More on JSX later)

If your component returns more than one line of code you must wrap it in parenthesis, like in the following example:

```javascript
export default function MyFirstComponent() {
  return (
    <>
      <h1>My First Component</h1>
      <p>Hello world</p>
    </>
  );
}
```

### Reusable components

Now that we have created our first React component, we can nest it inside others as many times as we want.

```javascript
function MyFirstComponent() {
  return <p>Hello world</p>;
}

export default function MyComponents() {
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
A component can have several child components, and at the same time those may have child components themselves. Just like in HTML the nesting of components is limitless.

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
import MyFirstComponent from "./MyFirstComponent.js";

export default function App() {
  return <MyFirstComponent />;
}
```

You may see some code that leaves off the .js file extension in the import, and that will work with React, though the former is closer to how native ES Modules work.

> _Note:_ There are two primary ways to export values with JavaScript: default exports and named exports. So far, our examples have only used default exports. But you can use one or both of them in the same file. A file can have no more than one default export, but it can have as many named exports as you like. This will affect how you import the file. Checkout this [MDN doc](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#description) for more info.

## JSX

The official React docs define JSX as _"a syntax extension for JavaScript that lets you write HTML-like markup inside a JavaScript file."_

You can still write vanilla Javascript to create a div or to add a title to your component, but what JSX allows you to do is to continue using the HTML markup we know, adding the ability to display dynamic content. That said JSX is not HTML, but rather it allows you to write HTML into a Javascript file, with a few more rules.

1. **Return a single root element:** All elements from a component, must be wrapped in a single parent tag. You can use an empty tag to avoid using unnecessary tags in your markup.

```javascript
<>
  <h1>My first Component</h1>
  <p>Hello world</p>
</>
```

> _Note:_ The empty tag is called [Fragment](https://react.dev/reference/react/Fragment) and it allows you to group elements without affecting the HTML markup.

2. **Close all the tags:** Even self-closing tags like < img > mas have a closing forward slash < img />.

3. **camelCase most things:** Because of Javascript limitations in variable names or reserved words like 'class', attribute names must be in camelCase and 'class' should be replaced with 'className'.

> _Note:_ Only [aria](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA) and [data](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/data-*) attributes are written with dashes (e.g., aria-label), as in HTML.

But the magic of JSX lies in the fact that even within your markup you can write JavaScript code. you may need to add logic or reference a dynamic property, and for that purpose you can use curly braces and open a JavScript window in your code.

```javascript
export default function TodoList() {
  const name = "Gregorio Y. Zara";
  return <h1>{name}'s To Do List</h1>;
}
```

> _Note:_ Any JavaScript expression will work between curly braces, including function calls

You can only use curly braces in two ways inside JSX:

1. **As text** inside a JSX tag:

```javascript
<h1> {name}'s To Do List </h1>
```

2. **As attributes** immediately following the = sign.

```javascript
src = { name };
```

> _Note:_ If you wish to pass in a JavaScript object you can use double curly braces, which is no special syntax, it’s just a JavaScript object tucked inside JSX curly braces.

## Props

### What are props?

We've been mentioning how React components are meant to be reusable, but what does that really implies? Let's look a the following example:

```javascript
function Card() {
  return (
    <div>
      <h3>Jhon Snow</h3>
      <p>Age: 31</p>
    </div>
  );
}
```

This is a valid React component, it can be imported into other components and be reused. But let's look at the output of doing so:

```javascript
function Card() {
  return (
    <div>
      <h3>Jhon Snow</h3>
    </div>
  );
}

export default function App() {
  return (
    <div className="App">
      <h1>Characters</h1>
      <h2>This is a list of characters from "A song of Ice and Fire"</h2>
      <Card />
      <Card />
      <Card />
    </div>
  );
}
```

If you ran the code, then you understand that we now have a problem: Our list of characters only includes Jhon Snow!
So you may be tempted to create a new component that returns, say Daenarys Targaryen, and then another for Jamie Lannister and so on. However you will have to copy paste all the styles and logic only to change the name of the character that's returned. Our component displays only static data and this goes against the idea of reusability.  
A better approach to this would be to use the same _Card_ component and make the name of the character dynamic, and that's where **Props** come in.  
Props are the way React components communicate with each other. Through them, parents can pass **any Javascript value** to a child component.

JSX tags can receive any of the [HTML standard](https://html.spec.whatwg.org/multipage/embedded-content.html#the-img-element) predefined props with information. However your custom components can receive any type of data through props.

In the following code the parent component _App_ is passing "name" props to the _Card_ child component.

```javascript
function Card({ name }) {
  return (
    <div>
      <p>{name}</p>
    </div>
  );
}

export default function App() {
  return (
    <div className="App">
      <h1>Characters</h1>
      <h2>This is a list of characters from "A song of Ice and Fire"</h2>
      <Card name="Jhon Snow" />
      <Card name="Daenarys Targaryen" />
      <Card name="Jamie Lannister" />
    </div>
  );
}
```

**Passing props**  
Any parent component can pass props to it's children, and they can be any Javascript value. To do so you can add them as properties in the JSX code, similar to HTML tag properties.

**Reading props**  
In the child component properties will be received in a Javascript object and they must be passed as a parameter in the function. As a convention we use the word _props_.

```javascript
function Card(props) {
  return (
    <div>
      <h4>{props.name}</h4>
      <ul>
        <li>House: {props.house}</li>
        <li>Age: {props.age}</li>
      </ul>
    </div>
  );
}

export default function App() {
  return (
    <div className="App">
      <h1>Characters</h1>
      <h2>This is a list of characters from "A song of Ice and Fire"</h2>
      <Card name="Jhon Snow" house="Stark" age={24} />
      <Card name="Daenerys Targaryen" house="Targaryen" age={18} />
      <Card name="Jamie Lannister" house="Lannister" age={43} />
    </div>
  );
}
```

_Exercise:_ Try running the code and add a console.log(props) in the line above the return. Check what was logged in the console, then try changing the word "props" for any word, has the log in the console changed?

You may have noticed we removed the curly braces around the _props_ parameter. This is because we receive the object and then access its properties with the dot operator.  
Another possibility is to use [destructuring for the object](https://www.freecodecamp.org/news/destructuring-patterns-javascript-arrays-and-objects/) in the following way:

```javascript
function Card({ name, house, age }) {
  return (
    <div>
      <h3>{name}</h3>
      <ul>
        <li>House: {house}</li>
        <li>Age: {age}</li>
      </ul>
    </div>
  );
}

export default function App() {
  return (
    <div className="App">
      <h1>Characters</h1>
      <h2>This is a list of characters from "A song of Ice and Fire"</h2>
      <Card name="Jhon Snow" house="Stark" age={24} />
      <Card name="Daenerys Targaryen" house="Targaryen" age={18} />
      <Card name="Jamie Lannister" house="Lannister" age={43} />
    </div>
  );
}
```

> _Remember:_ It's all Javascript under the hood.

### Props default value

React allows you to set a **default value** to fall back on when none is specified. To do so, use destructuring and add a value using the assignment operator.  
If no value is provided or the value is undefined for that property, then the default value will be used. If you want to avoid setting the default, but don't want to pass any value, you can set it to _0_ or _null_.

```javascript
function Card({ name, house, age = 50 }) {
  return (
    <div>
      <h3>{name}</h3>
      <ul>
        <li>House: {house}</li>
        <li>Age: {age}</li>
      </ul>
    </div>
  );
}

export default function App() {
  return (
    <div className="App">
      <h1>Characters</h1>
      <h2>This is a list of characters from "A song of Ice and Fire"</h2>
      <Card name="Jhon Snow" house="Stark" />
      <Card name="Daenerys Targaryen" house="Targaryen" age={undefined} />
      <Card name="Jamie Lannister" house="Lannister" age={null} />
    </div>
  );
}
```

### Props as children

If your intention is to nest content in a JSX tag, you can pass props and the child component will receive the content in the _children_ property.

```javascript
function Form(props) {
  return (
    <ul>
      <li>House: {props.house}</li>
      <li>Age: {props.age} </li>
    </ul>
  );
}

function Card(props) {
  return (
    <div>
      <h3>{props.name}</h3>
      {props.children}
    </div>
  );
}

export default function App() {
  return (
    <div className="App">
      <h1>Character profile</h1>
      <Card name="Jhon Snow">
        <Form house="Stark" age={24} />
      </Card>
    </div>
  );
}
```

> _Note:_ Props are immutable! They cannot be changed. However you may need to change a prop value, to do so the parent component will need to pass new props (different props) and the old ones will be discarded. This is handled through _State_, and will learn more about that soon.

## Class based components

> _Note:_ Before React 16.8, Class components were the only way to track state and lifecycle on a React component. Function components were considered "state-less".

React has traditionally supported two main ways of defining components: **class-based components** and **functional components**. However, with the introduction of React Hooks, functional components have become the preferred way of defining components in React. 

### Class based vs Functional components

1. Syntax:

   - Class-based components are defined using ES6 classes and extend the `React.Component` class. They have a `render` method where you return the JSX that represents the component's UI.

   ```javascript
   class MyComponent extends React.Component {
     render() {
       return <div>Hello, World!</div>;
     }
   }
   ```

2. State Management:

   Class components can manage state using `this.state` and `this.setState()`. They also have lifecycle methods like `componentDidMount`, `componentDidUpdate`, etc., for side effects and state updates.

   ```javascript
   class Counter extends React.Component {
     constructor() {
       super();
       this.state = { count: 0 };
     }

     increment() {
       this.setState({ count: this.state.count + 1 });
     }

     render() {
       return (
         <div>
           <p>Count: {this.state.count}</p>
           <button onClick={() => this.increment()}>Increment</button>
         </div>
       );
     }
   }
   ```

3. Lifecycle Methods:

   - Class components have lifecycle methods like `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount` for handling side effects and component updates.

   ```javascript
   componentDidMount() {
     // Called after the component is mounted to the DOM
   }

   componentDidUpdate(prevProps, prevState) {
     // Called after component updates with new props or state
   }

   componentWillUnmount() {
     // Called before the component is removed from the DOM
   }
   ```

4. Code Reusability:

   - **Functional Components:** Functional components are more lightweight and promote better code reusability through component composition and the use of custom hooks.

5. Readability and Conciseness:

   - **Functional Components:** Functional components tend to have a more concise and readable syntax, which can make the code easier to understand and maintain.

6. Performance:

   - In terms of performance, there is generally no significant difference between class-based and functional components. React optimizes both approaches.

7. Hooks:
   - Functional components are the preferred way to use React Hooks, such as `useState`, `useEffect`, `useContext`, etc. Hooks provide a more flexible and composable way to manage state and side effects in functional components.

In summary, while class-based components are still supported in React, functional components with hooks are the recommended and more modern way to build components due to their simplicity, code readability, and enhanced state management capabilities.
