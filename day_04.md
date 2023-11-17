# [A walk in React](/README.md)

## DAY 4

- [A walk in React](#a-walk-in-react)
  - [DAY 4](#day-4)
  - [Rendering Techniques](#rendering-techniques)
    - [Conditional Rendering](#conditional-rendering)
    - [Rendering lists](#rendering-lists)
    - [Renderdin data from arrays](#renderdin-data-from-arrays)
    - [Filtering data](#filtering-data)
    - [Key prop](#key-prop)
  - [Styling React components](#styling-react-components)
    - [Inline style](#inline-style)
    - [CSS stylesheet](#css-stylesheet)
    - [Dynamic styles (CSS-in-JS)](#dynamic-styles-css-in-js)
    - [CSS modules](#css-modules)
  - [Events](#events)
    - [Event handlers](#event-handlers)

## Rendering Techniques

### Conditional Rendering

In React you can conditionally render JSX components using Javascript syntax. An option would be to use an if statement in the following way:

```javascript
function Item({ name, isDone }) {
  if (isDone) {
    return <li className="item">{name} ✔</li>;
  }
  return <li className="item">{name}</li>;
}

export default function TodoList() {
  return (
    <section>
      <h1>Todo List</h1>
      <ul>
        <Item isDone={true} name="Buy milk" />
        <Item isDone={true} name="Study for test" />
        <Item isDone={false} name="Paint garage" />
      </ul>
    </section>
  );
}
```

> _Note:_ In case nothing should be returned if the condition is not met, you can use _null_. Try running the code, changing the _isDone_ property or returning null if _isDone_ is true.

But the [ternary operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) offers a more compact and easy to read syntax for this purpose.

```javascript
function Item({ name, isDone }) {
  return <li className="item"> {isDone ? name + " ✔" : name}</li>;
}

export default function TodoList() {
  return (
    <section>
      <h1>Todo List</h1>
      <ul>
        <Item isDone={true} name="Buy milk" />
        <Item isDone={true} name="Study for test" />
        <Item isDone={false} name="Paint garage" />
      </ul>
    </section>
  );
}
```

You can read it as _“if isDone is true, then render name + ' ✔', otherwise render name”._

Another option is to use the [logical AND (&&)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND) operator, which will evaluate the conditions from left to right and return the first [falsy value](https://www.freecodecamp.org/news/falsy-values-in-javascript/), or the last truthy, if they are all truthy. This is also known as short circuit evaluation.

Consider the following example:

```javascript
function Item({ name, isDone }) {
  return (
    <li className="item">
      {name} {isDone && "✔"}
    </li>
  );
}

export default function TodoList() {
  return (
    <section>
      <h1>To Do List</h1>
      <ul>
        <Item isDone={true} name="Space suit" />
        <Item isDone={true} name="Helmet with a golden leaf" />
        <Item isDone={false} name="Photo of Tam" />
      </ul>
    </section>
  );
}
```

The '✔' is visible only when _isDone_ is true, but if it is false then the '✔' is never evaluated.

_Note:_ Be aware of using numbers on the left side of AND (&&) operators, like in the following example:

```javascript
function Item({ counter }) {
  return counter && <h1>Hello world</h1>;
}

export default function Example() {
  return (
    <section>
      <Item counter={0} />
    </section>
  );
}
```

Run the code and try changing the value of counter. If _counter_ is 0 then the h1 tag will not be evaluated and the operator returns 0 (the first falsy condition).

A good rule of thumb regarding conditional rendering of components in React could be to use a ternary operator when you need to choose between one or the other (wheather is a component, class or element) and the AND operator to check wheather a component is render or not.
But still they are all valid options and it's best to decide what fits better in each scenario, and attempting to be consistent in the its use throughout the project.

### Rendering lists

Whilst creating your React app you will often need to display lists of data, or simply rendering the same component multiple times with different props.
To achieve this you can use Javascript mapping function and create JSX code on each element you map.

### Renderdin data from arrays

We can use the [map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) method to take regular Javascript data and turn it into JSX elements that can be rendered in out app.

```javascript
export default function TravelList() {
  const destinations = [
    {
      city: "London",
      country: "England",
      population: "8.982 million",
      area: 1572,
    },
    {
      city: "New York",
      country: "USA",
      population: "8.468 million",
      area: 783.8,
    },
    {
      city: "Athens",
      country: "Greece",
      population: "3.154 million",
      area: 38.96,
    },
    {
      city: "Rome",
      country: "Italy",
      population: "2.873 million",
      area: 1285,
    },
  ];

  const travelList = destinations.map((destination) => (
    <div>
      <h3>{destination.city}</h3>
      <ul>
        <li>Country: {destination.country}</li>
        <li>Population: {destination.population}</li>
        <li>Area: {destination.area + " km²"}</li>
      </ul>
    </div>
  ));

  return (
    <div>
      <h1>Travel List</h1>
      {travelList}
    </div>
  );
}
```

> What happens if you add or remove an element from the list?

As a result of using the map method we get cleaner code and a more efficient way of displaying data.

### Filtering data

But what if we only want to display cities with an area below 1.000 km²? In that case we can use the javascript [filter()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) method and subsequently map the what was returned.

```javascript
export default function TravelList() {
  const destinations = [
    {
      city: "London",
      country: "England",
      population: "8.982 million",
      area: 1572,
    },
    {
      city: "New York",
      country: "USA",
      population: "8.468 million",
      area: 783.8,
    },
    {
      city: "Athens",
      country: "Greece",
      population: "3.154 million",
      area: 38.96,
    },
    {
      city: "Rome",
      country: "Italy",
      population: "2.873 million",
      area: 1285,
    },
  ];

  const areaUnderThousand = destinations.filter(
    (destination) => destination.area < 1000
  );

  const travelList = areaUnderThousand.map((destination) => (
    <div>
      <h3>{destination.city}</h3>
      <ul>
        <li>Country: {destination.country}</li>
        <li>Population: {destination.population}</li>
        <li>Area: {destination.area + " km²"}</li>
      </ul>
    </div>
  ));

  return (
    <div>
      <h1>Travel List</h1>
      {travelList}
    </div>
  );
}
```

> Remember it's all Javascript under the hood

### Key prop

When you ran the examples above you most likely noticed an error in the Console:

> Warning: Each child in a list should have a unique “key” prop.

This is because React requires you to provide a unique identifier to each item on the array, to avoid errors if some data where to be deleted or edited. Usually the data we fetch will have it's own unique id, and the way to use it is to give each array item a _key_ prop with the unique id.

```javascript
export default function TravelList() {
  const destinations = [
    {
      city: "London",
      country: "England",
      population: "8.982 million",
      area: 1572,
      id: 1,
    },
    {
      city: "New York",
      country: "USA",
      population: "8.468 million",
      area: 783.8,
      id: 2,
    },
    {
      city: "Athens",
      country: "Greece",
      population: "3.154 million",
      area: 38.96,
      id: 3,
    },
    {
      city: "Rome",
      country: "Italy",
      population: "2.873 million",
      area: 1285,
      id: 4,
    },
  ];

  const areaUnderThousand = destinations.filter(
    (destination) => destination.area < 1000
  );

  const travelList = areaUnderThousand.map((destination) => (
    <div key={destination.id}>
      <h3>{destination.city}</h3>
      <ul>
        <li>Country: {destination.country}</li>
        <li>Population: {destination.population}</li>
        <li>Area: {destination.area + " km²"}</li>
      </ul>
    </div>
  ));

  return (
    <div>
      <h1>Travel List</h1>
      {travelList}
    </div>
  );
}
```

In the container div where we map the filtered cities we added the _key_ property and passed the id value onto it.

## Styling React components

There are many ways you can style your react components, and whilst React doesn't officially recommend a specific one, we will go over some of the most popular methods used and some of the pros and cons of each of them. These methods can be used simultaneously, but they may interfere with one another so it's important to plan ahead and document your styling choices.

> _Note:_ Remember that ultimately we are always writing CSS, the difference lies in the approaches we take.

### Inline style

---

React supports inline styling with JSX. You can add the style property in the tag and write the styles inside a Javascript object. The keys should be written using camelCase, and the values if they are pixels you can use numbers, but if you need to specify the measurement unit such as 'rem' or 'vh' the value mus be a string.

> _Note:_ Remember that JSX uses the curly braces to evaluate Javascript expressions so the syntax will be {{}}.

```javascript
export default function Card() {
  return (
    <div style={{ padding: "1rem", border: "1px solid red" }}>
      <h1>Jhon Doe</h1>
      <p>Developer</p>
      <p>Id: 000555444333</p>
    </div>
  );
}
```

To improve upon the code readability you can always extract the JS object.

```javascript
const cardStyles = {
  padding: "1rem",
  border: "1px solid red",
};

const infoStyles = {
  fontSize: "12px",
};

export default function Card() {
  return (
    <div style={cardStyles}>
      <h1>Jhon Doe</h1>
      <p>Developer</p>
      <p style={infoStyles}>Id: 000555444333</p>
    </div>
  );
}
```

_Pros_

- Scoped styles
- Easy to apply dynamic styling
- It avoids styles specificity conflicts as the styles are applied to a specific tag in separate components.
- It's easy to get rid of dead code, since it's in smaller files with less lines to go through.

_Cons_

- Must use Javascript syntax which reduces the power of CSS.
- Losing access to media queries, keyframe animations, pseudo selectors.
- No cascading capabilities.

Inline styling is not recommended as the primary means of styling projects, it's lower performant than adding classes and it decreases the readaibily of the code.

### CSS stylesheet

---

A more performant option is adding a stylesheet by creating a new .css file in your project directory and importing it into the application.
This approach possibly the most accessible one since all front end developers know how to write CSS so there is really no new tool to learn, and it requires no dependencies since it has native browser support. Still you may run into issues if your project is of medium or large scael or if you are collaborating with other developers.  
In order to add classes to a component you must use _className_, since class is a Javascript reserved word.

```javascript
import "./style.css";

export default function Card() {
  return (
    <div className="card">
      <h1>Jhon Doe</h1>
      <p>Developer</p>
      <p>Id: 000555444333</p>
    </div>
  );
}
```

**Pros**

- It's easy and fast to implement.
- Known CSS syntax.
- You can configure your bundle to use pre-processors like SASS or LESS.

But CSS in it's conception was meant to style a particular HTML document and not different components in the same application. the styles get imported into the HTML head tag and that brings the following consequences:

**Cons**

- The scope of the styles is global and as the project grows in scope the styles interfere with one another.
- Dead code elimination becomes more difficult as files grow in sizes.
- Even when creating several css files they will all get builed up into the same global scope.

In order to avoid this you can use some workarounds like [BEM Naming convention](<https://en.bem.info/methodology/naming-convention/#:~:text=%2D%2Dmod%2Dval-,Names%20are%20written%20in%20lowercase%20Latin%20letters.,a%20double%20hyphen%20(%20%2D%2D%20).>) whose purpose is to give names meaning so that they are as informative as possible for the developer and at the same time it avoids class names colliding with each other.

### Dynamic styles (CSS-in-JS)

---

This styling method can be achieved using libraries like: [JSS](https://cssinjs.org/?v=v10.10.0), [Emotion](https://emotion.sh/docs/introduction), [Styled components](https://styled-components.com/) or many others.

At there core this libraries allow us to use CSS in our React components, achieving a component with styling within the same file. They implement it differently so each one has specific documentation.

**Pros**

- Scope styling to specific components.
- Access to pseudo selectors, media queries and animations.
- Easy to achieve dynamic styling based on the state of the application.
- Some of the use CSS syntax.
- Some have pre built patterns which solve common problems like application theming.

**Cons**

- Styles need to be loaded, parsed and executed at runtime which makes it slower and increases the time-to-action.
- Styles are defined in the same component file. (Though this is personal preference)
- Need to learn new syntax.

This styling method is a good approach, but it may affect the performance of your application. Here are some articles on the matter if you wish to learn more about it, but keep in mind it will all depend on many factors, and it's always a matter of finding the right tool for each particular project.

[CSS vs CSS-in-JS Performance](https://medium.com/@pitis.radu/css-vs-css-in-js-performance-bcbdf8e1f6ff)  
[Why We're Breaking Up with CSS-in-JS](https://dev.to/srmagura/why-were-breaking-up-wiht-css-in-js-4g9b)

### CSS modules

---

CSS Module is a CSS file in which all class names and animation names are scoped locally by default tho the component that imports the CSS file.

The magic of CSS modules happens at build time when the local class names are mapped to custom automatically-generated ones and exported as a JS object literal to use within React.

To apply this approach you will need to create a CSS file and name it
ComponentName.module.css.
With in the component you will need to import the file and apply the styles in the _className_ property as a Javascript object.

```javascript
import styles from ".Button.module.css";

const Button = () => {
  return <button className={styles.error}></button>;
};
```

> _Note:_ You can use the dot notation to access Javascript object keys. In the example above _styles_ is the object the styles are bundled to.

**Pros:**

- No performance impact.
- Scoped styles.
- CSS syntax.
- Supports pre-processors.
- Easy to eliminate dead code, since they are all in separate files.

**Cons:**

- We have no way to share constants since CSS and JS are divided.
- CamelCase naming for CSS classes.
- Sharing styles between components and/or with global styles can be difficult.

This is a frequently used approach with great benefits and little downsides.

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
};
```
