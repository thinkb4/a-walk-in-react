# [A walk in React](/README.md)

## DAY 2

- [DAY 2](#day-2)
    - [Components](#components)
        - [My first component](#my-first-component)
        - [Reusable components](#reusable-components)
        - [JSX](#jsx)
    - [Props](#props)
      - [What are props](#what-are-props)
      - [Props default value](#props-default-value)
      - [Props as children](#props-as-children)
    - [Conditional rendering](#conditional-rendering)
    - [Rendering lists](#rendering-lists)
      - [Renderdin data from arrays](#rendering-data-from-arrays)
      - [Filtering data](#filtering-data)
      - [Key prop](#key-prop)

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
 import MyFirstComponent from './MyFirstComponent.js';
 
 export default function App() {  
  return (  
    <MyFirstComponent />  
  );  
}
```

You may see some code that leaves off the .js file extension in the import, and that will work with React, though the former is closer to how native ES Modules work.

> *Note:* There are two primary ways to export values with JavaScript: default exports and named exports. So far, our examples have only used default exports. But you can use one or both of them in the same file. A file can have no more than one default export, but it can have as many named exports as you like. This will affect how you import the file. Checkout this [MDN doc](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import#description) for more info.


## JSX

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


## Props  

### What are props?

We've been mentioning how React components are ment to be reusable, but what does that really implies? Let's look a the following example:

```javascript
function Card () {
  return (
    <div>
      <h3>Jhon Snow</h3>
      <p>Age: 31</p>
    </div>
  )
}
```
This is a valid React component, it can be imported into other compontents and be reused. But let's look at the output of doing so: 

```javascript
function Card () {
  return (
    <div>
      <h3>Jhon Snow</h3>
    </div>
  )
}

export default function App() {
  return (
    <div className="App">
      <h1>Characters</h1>
      <h2>This is a list of characters from "A song of Ice and Fire"</h2>
      <Card/>
      <Card/>
      <Card/>
    </div>
  );
}
```
If you ran the code, then you undestand that we now have a problem: Our list of characters only includes Jhon Snow! 
So you may be tempted to create a new component that returns, say Daenarys Targaryen, and then another for Jamie Lannister and so on. However you will have to copy paste all the styles and logic only to change the name of the character that's returned. Our component displays only static data and this goes against the idea of reusability.  
A better approach to this would be to use the same *Card* component and make the name of the character dynamic, and that's where **Props** come in.  
Props are the way React components comunicate with each other. Through them, parents can pass **any Javascript value** to a child component.

JSX tags can recive any of the [HTML standard](https://html.spec.whatwg.org/multipage/embedded-content.html#the-img-element) predefined props with information. However your custom components can recive any type of data through props.

In the following code the parent component *App* is passing "name" props to the *Card* child component.  

```javascript
function Card ({name}) {
  return (
    <div>
      <p>{name}</p>
    </div>
  )
}

export default function App() {
  return (
    <div className="App">
      <h1>Characters</h1>
      <h2>This is a list of characters from "A song of Ice and Fire"</h2>
      <Card name="Jhon Snow"/>
      <Card name="Daenarys Targaryen"/>
      <Card name="Jamie Lannister"/>
    </div>
  );
}
```
**Passing props**  
Any parent component can pass props to it's children, and they can be any Javascript value. To do so you can add them as properties in the JSX code, similar to HTML tag properties.

**Reading props**  
In the child component properties will be recieved in a Javascript object and they must be passed as a parameter in the function. As a convetion we use the word *props*. 


```javascript
function Card (props) {
  return (
    <div>
      <h4>{props.name}</h4>
      <ul>
        <li>House: {props.house}</li>
        <li>Age: {props.age}</li>
      </ul>
    </div>
  )
}

export default function App() {
  return (
    <div className="App">
      <h1>Characters</h1>
      <h2>This is a list of characters from "A song of Ice and Fire"</h2>
      <Card 
        name="Jhon Snow"
        house="Stark"
        age={24}
      />
      <Card 
        name="Daenerys Targaryen"
        house="Targaryen"
        age={18}
      />
      <Card 
        name="Jamie Lannister"
        house="Lannister"
        age={43}
      />
    </div>
  );
}
```

*Exercise:* Try running the code and add a console.log(props) in the line above the return. Check what was logged in the console, then try changing the word "props" for any word, has the log in the console changed? 

You may have noticed we removed the curly braces around the *props* parameter. This is because we recieve the object and then access its properties with the dot operator.  
Another posibility is to use [destructuring for the object](https://www.freecodecamp.org/news/destructuring-patterns-javascript-arrays-and-objects/) in the following way: 

```javascript
function Card ({name, house, age}) {
  return (
    <div>
      <h3>{name}</h3>
      <ul>
        <li>House: {house}</li>
        <li>Age: {age}</li>
      </ul>
    </div>
  )
}

export default function App() {
  return (
    <div className="App">
      <h1>Characters</h1>
      <h2>This is a list of characters from "A song of Ice and Fire"</h2>
      <Card 
        name="Jhon Snow"
        house="Stark"
        age={24}
      />
      <Card 
        name="Daenerys Targaryen"
        house="Targaryen"
        age={18}
      />
      <Card 
        name="Jamie Lannister"
        house="Lannister"
        age={43}
      />
    </div>
  );
}
```

> *Remember:* It's all Javascript under the hood.

### Props default value 
React allows you to set a **default value** to fall back on when none is specified. To do so, use destructuring and add a value using the assignment operator.  
If no value is provided or the value is undefined for that propertie, then the default value will be used. If you want to avoid setting the default, but don't want to pass any value, you can set it to *0* or *null*.  

```javascript
function Card ({name, house, age = 50 }) {
  return (
    <div>
      <h3>{name}</h3>
      <ul>
        <li>House: {house}</li>
        <li>Age: {age}</li>
      </ul>
    </div>
  )
}

export default function App() {
  return (
    <div className="App">
      <h1>Characters</h1>
      <h2>This is a list of characters from "A song of Ice and Fire"</h2>
      <Card 
        name="Jhon Snow"
        house="Stark"
      />
      <Card 
        name="Daenerys Targaryen"
        house="Targaryen"
        age={undefined}
      />
      <Card 
        name="Jamie Lannister"
        house="Lannister"
        age={null}
      />
    </div>
  );
}
```

### Props as children

If your intention is to nest content in a JSX tag, you can pass props and the child component will recieve the content in the *children* propertie.

```javascript
function Form (props) {
  return (
    <ul>
      <li>House: {props.house}</li>
      <li>Age: {props.age} </li>
    </ul>
  )
}

function Card (props) {
  return (
    <div>
      <h3>{props.name}</h3>
      {props.children}
    </div>
  )
}

export default function App() {
  return (
    <div className="App">
      <h1>Character profile</h1>
      <Card name="Jhon Snow">
        <Form house="Stark" age={24}/>
      </Card>
    </div>
  );
}
```

> *Note:* Props are inmutable! They cannot be changed. However you may need to change a prop value, to do so the parent component will need to pass new props (different props) and the old ones will be discarded. This is handeled through *State*, and will learn more about that soon. 

## Conditional Rendering

In React you can conditionally render JSX components using Javascript syntax. An option would be to use an if statement in the following way:

````javascript
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
        <Item 
          isDone={true} 
          name="Buy milk" 
        />
        <Item 
          isDone={true} 
          name="Study for test" 
        />
        <Item 
          isDone={false} 
          name="Paint garage" 
        />
      </ul>
    </section>
  );
}
````

>*Note:* In case nothing should be returned if the condition is not met, you can use *null*. Try running the code, changing the *isDone* property or returning null if *isDone* is true. 

But the [ternary operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Conditional_Operator) offers a more compact and easy to read syntax for this purpose.

````javascript
function Item({ name, isDone }) {
    return <li className="item"> {isDone ? name + ' ✔' : name}</li>;
}

export default function TodoList() {
  return (
    <section>
      <h1>Todo List</h1>
      <ul>
        <Item 
          isDone={true} 
          name="Buy milk" 
        />
        <Item 
          isDone={true} 
          name="Study for test" 
        />
        <Item 
          isDone={false} 
          name="Paint garage" 
        />
      </ul>
    </section>
  );
}
````
You can read it as *“if isDone is true, then render name + ' ✔', otherwise render name”.*

Another option is to use the [logical AND (&&)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND) operator, which will evaluate the conditions from left to right and return the first [falsy value](https://www.freecodecamp.org/news/falsy-values-in-javascript/), or the last truthy, if they are all truthy. This is also known as short circuit evaluation.

Consider the following example:

````javascript
function Item({ name, isDone }) {
  return (
    <li className="item">
      {name} {isDone && '✔'}
    </li>
  );
}

export default function TodoList() {
  return (
    <section>
      <h1>To Do List</h1>
      <ul>
        <Item 
          isDone={true} 
          name="Space suit" 
        />
        <Item 
          isDone={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isDone={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
````

The '✔' is visible only when *isDone* is true, but if it is false then the '✔' is never evaluated.

*Note:* Be aware of using numbers on the left side of AND (&&) operators, like in the following example:

````javascript
function Item({ counter }) {
  return (
    counter && <h1>Hello world</h1>
  );
}

export default function Example() {
  return (
    <section>
        <Item 
          counter={0} 
        />

    </section>
  );
}
````
 Run the code and try changing the value of counter. If *counter* is 0 then the h1 tag will not be evaluated and the operator returns 0 (the first falsy condition). 

 A good rule of thumb regarding conditional rendering of components in Recat could be to use a ternary operator when you need to choose between one or the other (wheater is a component, class or element) and the AND operator to check wheather a component is render or not.
 But still they are all valid options and it's best to decide what fits better in each scenario, and attempting to be consistent in the its use throughout the project.  


## Rendering lists

Whilst creating your React app you will often need to display lists of data, or simply rendering the same component multiple times with different props. 
To achive this you can use Javascript mapping function and create JSX code on each element you map.    
### Renderdin data from arrays

We can use the [map()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) method to take regular Javascript data and turn it into JSX elements that can be rendered in out app. 

````javascript
export default function TravelList() {

  const destinations = [
    {
      city: 'London',
      country: 'England',
      population: '8.982 million',
      area: 1572
    },
    {
      city: 'New York',
      country: 'USA',
      population: '8.468 million',
      area: 783.8
    },
    {
      city: 'Athens',
      country: 'Greece',
      population: '3.154 million',
      area: 38.96 
    },
    {
      city: 'Rome',
      country: 'Italy',
      population: '2.873 million',
      area: 1285
    } 
  ]

  const travelList = destinations.map(destination => (
    <div>
      <h3>{destination.city}</h3>
      <ul>
        <li>Country: {destination.country}</li>
        <li>Population: {destination.population}</li>
        <li>Area: {destination.area + ' km²'}</li>
      </ul>
    </div>
   ))

  return (
    <div>
      <h1>Travel List</h1>
      {travelList}
    </div>
  );
}
````

> What happenes if you add or remove an element from the list? 

As a result of using the map method we get cleaner code and a more efficient way of displaying data. 

### Filtering data

But what if we only want to display cities with an area below 1.000 km²? In that case we can use the javascript [filter()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter) method and subsequently map the what was returned. 

````javascript 
export default function TravelList() {

  const destinations = [
    {
      city: 'London',
      country: 'England',
      population: '8.982 million',
      area: 1572
    },
    {
      city: 'New York',
      country: 'USA',
      population: '8.468 million',
      area: 783.8
    },
    {
      city: 'Athens',
      country: 'Greece',
      population: '3.154 million',
      area: 38.96 
    },
    {
      city: 'Rome',
      country: 'Italy',
      population: '2.873 million',
      area: 1285
    } 
  ]

  const areaUnderThousand = destinations.filter(destination => destination.area < 1000)

  const travelList = areaUnderThousand.map(destination => (
    <div>
      <h3>{destination.city}</h3>
      <ul>
        <li>Country: {destination.country}</li>
        <li>Population: {destination.population}</li>
        <li>Area: {destination.area + ' km²'}</li>
      </ul>
    </div>
   ))
   
  return (
    <div>
      <h1>Travel List</h1>
      {travelList}
    </div>
  );
}

````

> Remember it's all Javascript under the hood

### Key prop

When you ran the exaples above you most likely noticed an error in the Console: 
> Warning: Each child in a list should have a unique “key” prop.

This is because React requires you to provide a unique identifier to each item on the array, to avoid errors if some data where to be deleted or edited. Usually the data we fetch will have it's own unique id, and the way to use it is to give each array item a *key* prop with the unique id.

````javascript 
export default function TravelList() {

  const destinations = [
    {
      city: 'London',
      country: 'England',
      population: '8.982 million',
      area: 1572,
      id: 1
    },
    {
      city: 'New York',
      country: 'USA',
      population: '8.468 million',
      area: 783.8,
      id: 2
    },
    {
      city: 'Athens',
      country: 'Greece',
      population: '3.154 million',
      area: 38.96 ,
      id: 3
    },
    {
      city: 'Rome',
      country: 'Italy',
      population: '2.873 million',
      area: 1285,
      id: 4
    } 
  ]

  const areaUnderThousand = destinations.filter(destination => destination.area < 1000)

  const travelList = areaUnderThousand.map(destination => (
    <div key={destination.id}>
      <h3>{destination.city}</h3>
      <ul>
        <li>Country: {destination.country}</li>
        <li>Population: {destination.population}</li>
        <li>Area: {destination.area + ' km²'}</li>
      </ul>
    </div>
   ))
   
  return (
    <div>
      <h1>Travel List</h1>
      {travelList}
    </div>
  );
}

````

In the container div where we map the filtered cities we added the *key* property and passed the id value onto it.  




