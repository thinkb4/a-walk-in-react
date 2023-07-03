# [A walk in React](/README.md)

## DAY 5

- [DAY 5](#day-5)
  - [React Context API](#react-context-api)
    - [createContext](#createcontext)
    - [useContext](#usecontext)
    - [Context Provider](#context-provider)

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
