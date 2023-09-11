# [A walk in React](/README.md)

## DAY 1

- [A walk in React](#a-walk-in-react)
  - [DAY 1](#day-1)
  - [Introduction of React](#introduction-of-react)
    - [What is ReactJS?](#what-is-reactjs)
    - [Main features of React](#main-features-of-react)
    - [What does it mean to be declarative?](#what-does-it-mean-to-be-declarative)
  - [Setup](#setup)
    - [Requirements](#requirements)
    - [Create-react-app](#create-react-app)
    - [Vite](#vite)
    - [Recommended extensions for VSCode](#recommended-extensions-for-vscode)
  - [Create-react-app(CRA) vs Vite](#create-react-appcra-vs-vite)

## Introduction of React

### What is ReactJS?

React is an open source JavaScript library for building user interfaces. It is based on UI componentization: the interface is divided into independent components, which contain their own state. When the state of a component changes, React re-renders the interface.
This makes React a very useful tool for building complex interfaces, as it allows you to break the interface down into smaller, reusable pieces.

### Main features of React

- **Components**: React is based on the componentization of the UI. The interface is divided into independent components, which contain their own state. When the state of a component changes, React re-renders the interface.

- **Virtual DOM**: React uses a virtual DOM to render components. The virtual DOM is an in-memory representation of the real DOM. When the state of a component changes, React re-renders the interface. Instead of modifying the actual DOM, React modifies the virtual DOM and then compares the virtual DOM with the actual DOM. This way, React knows what changes should be applied to the actual DOM.

- **Declarative**: React is declarative, which means that you don't specify how a task should be performed, but rather what should be performed. This makes the code easier to understand and maintain.

- **Unidirectional**: React is unidirectional, which means that data flows in only one direction. Data flows from parent components to child components.

- **Universal**: React can be run on both the client and the server. Also, you can use React Native to create native apps for Android and iOS.

### What does it mean to be declarative?

We don't tell you how to render the command-based interface. We tell it what to render and React takes care of rendering it.

An example between declarative and imperative:

```javascript
// Declarative
const element = <h1>Hello, world</h1>;

// Imperative
const element = document.createElement("h1");
element.innerHTML = "Hello, world";
```

## Setup

### Requirements

- **NodeJS**: Is an open source, cross-platform runtime environment for (but not limited to) the server layer based on the JavaScript programming language.

- **NPM** (Node Package Manager): Is a package manager developed entirely under the JavaScript language by Isaac Schlueter, through which we can obtain any library with just a simple line of code, which will allow us to add dependencies in a simple way, distribute packages and efficiently manage both the modules and the project to be developed in general. There are other package managers like **YARN** and **PNPM**.

### Create React App

Facebook (Meta) has created the [Create React App](https://create-react-app.dev), an environment that comes preconfigured with everything you need to create a React app. You don't need to install or configure tools like webpack or Babel. They are preconfigured and hidden so you can focus on the code.

```
npx create-react-app my-app
cd my-app
npm start
```

> _Note:_ Create React App CLI is no longer recommended to use, since its latest version is from April 2022.
> React development team recently removed create-react-app from the official documentation. This means it is no longer the default method of setting up a new project in React. According to [this](https://github.com/reactjs/react.dev/pull/5487) pull request on Github, create-react-app is finally gone.

**What are the alternatives?**

There are multiple approaches to initiate a new React project. Notably, the official documentation now highlights frameworks such as Next.js, Remix, and more. In this guide, we will explore the quick and efficient process of using Vite to set up our React application in under a minute.

### Vite

[Vite](https://vitejs.dev) is a build tool that aims to provide a faster and more agile development experience for modern web projects. It consists of two main parts:

- A development server that provides rich functionality enhancements over native ES modules, for example extremely fast Hot Module Replacement (HMR).

- A build command that packages your code with Rollup, preconfigured to generate highly optimized static resources for production.

**Creating a new React project using Vite**

Let's create a new React project using Vite. Run the following in the folder where you want your new app.

- With NPM:

```
npm create vite@latest
```

- With Yarn:

```
yarn create vite
```

- With PNPM:

```
pnpm create vite
```

### Recommended extensions for VSCode

While there isn't a specific extension required to work with React, here are some recommendations to enhance your workflow

- [ES7 React Js Snippets](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)
- [Prettier](https://marketplace.visualstudio.com/items?itemName=esbenp.prettier-vscode)
- [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)

## Create-react-app(CRA) vs Vite

Vite and CRA are not as different as you might think. They basically do more or less the same thing, which is serve a local development server and bundle code for production. The main difference you'll notice is how the code is served in development and which modules are supported. Vite doesn't need to bundle the entire application or transpile the modules and code before starting a development server, transpilation is done on demand, making it significantly faster than CRA.

Additionally, Vite provides the option to install from [community maintained templates](https://github.com/vitejs/awesome-vite#templates) (like Vanilla, Vanilla TS, React, React TS, Vue, Vue TS, etc.) or pre-configure TypeScript and SWR during the installation process.