# [A walk in React](/README.md)

## DAY 1

- [A walk in React](#a-walk-in-react)
  - [DAY 1](#day-1)
  - [Introduction of React](#introduction-of-react)
    - [What is React.JS?](#what-is-reactjs)
    - [Main features of React:](#main-features-of-react)
    - [What exactly does it mean to be declarative?](#what-exactly-does-it-mean-to-be-declarative)
  - [Setup](#setup)
    - [Requirements](#requirements)
    - [Extensions VSCode](#Extensions-VSCode)
    - [Vite](#vite)
    - [Create-react-app](#create-react-app)
  - [Create-react-app(CRA) vs Vite](#create-react-appcra-vs-vite)

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

## Setup

### Requirements

- **Node.JS**: Is an open source, cross-platform runtime environment for the server layer (but not limited to) based on the JavaScript programming language.
- **NPM**(Node Package Manager): Is a package manager developed entirely under the JavaScript language by Isaac Schlueter, through which we can obtain any library with just a simple line of code, which will allow us to add dependencies in a simple way, distribute packages and efficiently manage both the modules and the project to be developed in general.

### Extensions VSCode
- [es7 react js snippets](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)
- [material icon theme](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme)
- [nu-disco theme](https://marketplace.visualstudio.com/items?itemName=dbanksdesign.nu-disco)


### Vite
- [Vite official website](https://vitejs.dev) : Vite is defined as a frontend tool that will help you create projects (without tying you to any specific framework) and make their development and final construction as easy as possible.
- Currently, Vite supports both vanilla projects (without using frameworks), and projects using Vue, React, Preact or Lit-element (both in Javascript and Typescript versions).

```
npm create vite@latest
```

### Create-react-app
- [create-react-app](https://create-react-app.dev): Fortunately, Facebook has created the Create React App, an environment that comes preconfigured with everything you need to create a React app.
- You will create a live development server.
- No need to install or configure tools like webpack or Babel. They are preconfigured and hidden so you can focus on the code.
- Advantage: Focus on the code, not the build tools.

```
npx create-react-app my-app
cd my-app
npm start
```

## Create-react-app(CRA) vs Vite

- Vite and CRA are not as different as you might think. They basically do more or less the same thing, which is serve a local development server and bundle code for production.
- The main difference you'll notice is how the code is served in development and which modules are supported.
- Vite doesn't need to bundle the entire application or transpile the modules and code before starting a development server; transpilation is done on demand, making it significantly faster than CRA.