# [A walk in React](/README.md)

## DAY 1

- [A walk in React](#a-walk-in-react)
  - [DAY 1](#day-1)
  - [Introduction of React](#introduction-of-react)
    - [What is React.JS?](#what-is-reactjs)
    - [Main features of React:](#main-features-of-react)
    - [What exactly does it mean to be declarative?](#what-exactly-does-it-mean-to-be-declarative)

## Introduction of React

### What is React.JS?

React is an open source JavaScript library for building user interfaces. It is based on UI componentization: the interface is divided into independent components, which contain their own state. When the state of a component changes, React re-renders the interface.
This makes React a very useful tool for building complex interfaces, as it allows you to break the interface down into smaller, reusable pieces.

### Main features of React:

- Components: React is based on the componentization of the UI. The interface is divided into independent components, which contain their own state. When the state of a component changes, React re-renders the interface.

- Virtual DOM: React uses a virtual DOM to render components. The virtual DOM is an in-memory representation of the real DOM. When the state of a component changes, React re-renders the interface. Instead of modifying the actual DOM, React modifies the virtual DOM and then compares the virtual DOM with the actual DOM. This way, React knows what changes should be applied to the actual DOM.

- Declarative: React is declarative, which means that you don't specify how a task should be performed, but rather what should be performed. This makes the code easier to understand and maintain.

- Unidirectional: React is unidirectional, which means that data flows in only one direction. Data flows from parent components to child components.

- Universal: React can be run on both the client and the server. Also, you can use React Native to create native apps for Android and iOS.

### What exactly does it mean to be declarative?

We don't tell you how to render the command-based interface. We tell it what to render and React takes care of rendering it.

An example between declarative and imperative:

```javascript
// Declarative
const element = <h1>Hello, world</h1>

// Imperative
const element = document.createElement('h1')
element.innerHTML = 'Hello, world'
}
```