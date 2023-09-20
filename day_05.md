# [A walk in React](/README.md)

## DAY 5

- [DAY 5](#day-5)
  - [Forms in React](#forms-in-react)
  - [React Context API](#react-context-api)
    - [createContext](#createcontext)
    - [useContext](#usecontext)
    - [Context Provider](#context-provider)
  - [Global State Managment](#global-state-management)

## Forms in React

Working with **forms** in ReactJS is a little different than Vanilla Javascript. React encourages a "single source of truth" approach to state management. Form data is typically stored in the component's state, and any changes to the form inputs are reflected in the state immediately using the `onChange` event. React's unidirectional data flow ensures that the state remains consistent and helps manage form data efficiently. In vanilla JavaScript, you have the flexibility to manage form data and state in various ways. You could use global variables, local variables, or other custom data structures to handle form data changes. However, this approach might require extra effort to manage state consistently, especially in large applications.

React uses the **virtual DOM**, which is an in-memory representation of the actual DOM. When form inputs change, React efficiently updates only the necessary parts of the virtual DOM, and then, through a process called reconciliation, it updates the real DOM with the minimum required changes. But, with vanilla JavaScript, you need to manually manipulate the DOM to update form values and handle form events. This involves directly accessing DOM elements, reading input values, and updating the UI accordingly. This approach may lead to more verbose and error-prone code, especially for complex forms.

> Overall, ReactJS simplifies form handling by providing a declarative approach to UI development, efficient DOM updates, and state management out of the box. In contrast, working with
> forms in vanilla JavaScript requires more low-level DOM manipulation and state management.

Let's see an example of how to handle a Form with React:

```javascript
function MyForm() {
  const [formData, setFormData] = useState({
    name: "",
    email: "",
  });

  function handleChange(e) {
    const { name, value } = e.target;

    setFormData((prevData) => ({
      ...prevData,
      [name]: value,
    }));
  }

  return (
    <form>
      <input
        type="text"
        name="name"
        value={formData.name}
        onChange={handleChange}
        placeholder="Enter your name"
      />
      <input
        type="email"
        name="email"
        value={formData.email}
        onChange={handleChange}
        placeholder="Enter your email"
      />
    </form>
  );
}
```

In this example, we started by defining the initial state of the `Form Component` using **useState** hook. This state will hold the values of the form fields. Then, for each form field, we should attach an `onChange event handler` to update the corresponding state value as the user types in the input field. By using the onChange event handler and updating the state with the new values, the component will automatically re-render with the updated state, reflecting the user's input.

Let's add a form submission:

```javascript
function MyForm() {
  const [formData, setFormData] = useState({
    name: "",
    email: "",
  });

  function handleSubmit(e) {
    e.preventDefault();
    sendData(formData); // Or do anything with form data.
  }

  return (
    <form onSubmit={handleSubmit}>
      {/* Form fields */}

      <button type="submit">Submit</button>
    </form>
  );
}
```

To handle **form submission**, we attached an `onSubmit event handler` to the form element. This handler can send the form data to a server or perform any necessary actions based on the form data. It's important to note that the `submit event` will try to submit the Form by default, but in this case, we didn't specify an action property in the [`<form>`](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/form) tag. To prevent the default form submission behavior, we used the [preventDefault](https://developer.mozilla.org/es/docs/Web/API/Event/preventDefault) method. This ensures that the form data is processed as intended within our React application without triggering a full page reload.

## React Context API

In React, you will often find yourself transferring data from a parent component to a child component using props. However, this approach can become unwieldy and impractical when you need to pass props through multiple intermediate components or when several components in your application require the same data. This is where the concept of Context comes into play.

> Passing props is a great way to explicitly pipe data through your UI tree to the components that use it.
> But passing props can become verbose and inconvenient when you need to pass some prop deeply through the tree, or if many components need the same prop. The nearest common ancestor could be far removed from the components that need data, and lifting state up that high can lead to a situation called “prop drilling”.
> Source: [React Oficial Documentation](https://react.dev/learn/passing-data-deeply-with-context)

Context allows the parent component to share specific information with any component in its component tree, regardless of how deeply nested they are, without the need to pass it explicitly through props. By utilizing context, you can simplify the process of accessing shared data and eliminate the verbosity and inconvenience associated with passing props extensively.

Let's see an example from [React Oficial Documentation](https://react.dev/):

### createContext

First, you need to create the context. You’ll need to export it from a file so that your components can use it:

```javascript
import { createContext } from "react";

export const ThemeContext = createContext(null);
```

The only argument to createContext is the default value. You could pass any kind of value (even an object). You will see the significance of the default value in the next step.

### useContext

Import the useContext Hook from React and your context. Call useContext at the top level of your component to read and subscribe to context:

```javascript
import { useContext } from "react";

function Button() {
  const theme = useContext(ThemeContext);
  // ..
}
```

useContext returns the context value for the context you passed. To determine the context value, React searches the component tree and finds the closest context provider above for that particular context.

### Context Provider

To pass context to a Button, wrap it or one of its parent components into the corresponding context provider:

```javascript
function MyPage() {
  return (
    <ThemeContext.Provider value="dark">
      <Form />
    </ThemeContext.Provider>
  );
}

function Form() {
  // ... renders buttons inside ...
}
```

It doesn’t matter how many layers of components there are between the provider and the Button. When a Button anywhere inside of Form calls useContext(ThemeContext), it will receive "dark" as the value.

> useContext() always looks for the closest provider above the component that calls it. It searches upwards and does not consider providers in the component from which you’re calling useContext().
> Source: [React Oficial Documentation](https://react.dev/reference/react/useContext#usage)

Wrap your components into a context provider to specify the value of this context for all components inside:

```javascript
function MyPage() {
  const [theme, setTheme] = useState("dark");

  return (
    <ThemeContext.Provider value={theme}>
      <Form />
    </ThemeContext.Provider>
  );
}
```

> value: The value that you want to pass to all the components reading this context inside this provider, no matter how deep. The context value can be of any type. A component calling useContext(SomeContext) inside of the provider receives the value of the innermost corresponding context provider above it.
> Source: [React Oficial Documentation](https://react.dev/reference/react/createContext#provider)

To update context, combine it with state. Declare a state variable in the parent component, and pass the current state down as the context value to the provider.

```javascript
function MyPage() {
  const [theme, setTheme] = useState("dark");

  return (
    <ThemeContext.Provider value={theme}>
      <Form />

      <Button
        onClick={() => {
          setTheme("light");
        }}>
        Switch to light theme
      </Button>
    </ThemeContext.Provider>
  );
}
```

Now any Component inside of the provider will receive the current theme value. If you call setTheme to update the theme value that you pass to the provider, all Button components will re-render with the new 'light' value.

## Global State Management

Global state management in React refers to the management of shared state data that needs to be accessible and synchronized across multiple components within an application. In React, components typically manage their own local state using the **useState** hook or class-based component state. However, in larger applications or scenarios where multiple components need to access and modify the same data, managing state individually in each component can become complex and lead to prop drilling (passing down props through multiple levels).

As we saw below, [React Context API](#react-context-api) is a good option for global state management in a React application. But, if the application grows and evolves, it is easy for the state to become difficult to manage and maintain, which can lead to bugs and other issues. Thus, it is essential to have a strategy in place to manage data. Thanks to the development of React and new libraries, there are many state management libraries and approaches for React applications like Redux, MobX, Zustand, and more.

**Redux** is a popular state management library for React applications. It provides a predictable state container that helps manage the state of an application in a centralized manner. Redux follows a unidirectional data flow pattern, which means that data flows in a single direction, making it easier to track and manage state changes.

> Redux helps you manage and update the application state in a predictable and organized way. It keeps the state separate from your components and makes it easier to understand and control how data changes in your React application. **But it's important to understand that Redux, while a powerful tool, may introduce complexity that is not always necessary for every project**.

Here's a high-level overview of how Redux works in a React application:

Imagine your application's state as a single big box. This box holds all the data that your app needs to work. To manage that state, Redux introduces a special store. The **store** is like a supervisor that keeps track of the state box. When something happens in your app (like a button click or data received from a server), you create an action. An **action** is like a message that describes what happened. You send that action to the store by dispatching it. It's like telling the supervisor what just occurred.

The store receives the action and passes it to the reducer. A **reducer** is like a worker who knows how to update the state based on the action. The reducer takes the current state and the action, and produces a new state. The store updates the state with the new state produced by the reducer. It replaces the old state box with the new one. Now, any component that is interested in the state can **subscribe** to the store. When the state changes, the subscribed components are notified, and they can update themselves accordingly. Components can also send actions to the store to modify the state. It's like asking the supervisor to change the state box.

> As you can see, Redux implements a lot of concepts for its use. Redux often requires writing additional code for **actions**, **action creators**, and **reducers**. This can lead to an increase in **boilerplate code**, making the application more complex and harder to maintain. In smaller projects, the overhead of writing and managing this additional code may outweigh the benefits. Also, implementing Redux requires additional setup and configuration compared to local state management in React and adds extra dependencies to your project, which can increase the bundle size.

It's important to note that these points don't mean Redux is inherently bad or should never be used in smaller applications. Redux is a robust solution for managing complex state and is well-suited for larger-scale applications where the benefits outweigh the added complexity. However, for smaller projects, it's worth considering whether the additional overhead of implementing Redux is truly necessary, or if a simpler state management solution would suffice.
