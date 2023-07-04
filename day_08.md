# [A walk in React](/README.md)

## DAY 8

- [DAY 8](#day-8)
    - [React Router](#react-router)
        - [Installing React Router](#installing-react-router)
        - [Defining Routes](#defining-routes)
        - [Navigate between routes](#navigating)
        - [Error pages](#error-pages)
        - [Dynamic Routes](#dynamic-routes)
        - [Protected Routes](#protected-routes)

## React Router 

What is react router? Well, since react has no embeded route managment tools, we need to appeal to a third party library to do this, there are a few libraries that do this, but the most used is React Router. This library is a tool that allows you to handle routes in a web app, using commonly dynamic routing. Dynamic routing takes place as the app is rendering on your machine, unlike the old routing architecture where the routing is handled in a configuration outside of a running app. React router implements a component-based approach to routing. It provides different routing components according to the needs of the application and platform.

## Installing React Router

Installing React Router in our project is as simple as to run `npm install react-router-dom` or `yarn add react-router-dom`

## Defining Routes

In most common cases, in our `App.js` file is where the magic occurs. In this file is where we're gonna define our app routes. 

React Router provides us a few components to use, we're gonna focus on `<Switch/>`, `<Route/>` and `<Router/>` components. Which are the most commonly used on basic routing building. Later on we're gonna focus on private routing (which are routes that only authenticated users can access)

First of all, we need to import all the required components from `react-router-dom`
````javascript
    import {BrowserRouter as Router, Switch,Route,} from "react-router-dom";

    export default App(){
        return(
            <h1>We're learning Routes!</h1>
        )
    }

````

After we have imported all the components that React Router provides us let's start building our Routes. For creating the routes, we need to wrap our application with the `<Router/>` component. Inside the `<Router/>` component we can create our layout. In this example we have simple layout that's made with a `<NavBar/>` and `<Footer/>` components. Every component that's outside the `<Switch/>` component will remain untouched.  When a `<Switch>` is rendered, it searches through its children `<Route>` elements to find one whose path matches the current URL. When it finds one, it renders that `<Route>` and ignores all others.

````javascript
    import {BrowserRouter as Router, Switch,Route,} from "react-router-dom";
    import {About, Users, Home} from 'pages'

    export default App(){
        return(
           <Router> 
                <NavBar/>
                    <Switch>
                        <Route path="/about">
                            <About />
                        </Route>
                        <Route path="/users">
                            <Users />
                        </Route>
                        <Route path="/">
                            <Home />
                        </Route>
                    </Switch>
                <Footer/>
            <Router/>
        )
    }

````
Let's assume we just opened our app and the browser shows "https://yourAppDomain.com/" the route that matches with the current url is the third one, consecuentelly, `<Home/>` component will be rendered.  

## Navigate between routes

React Router provides a <Link> component to create links in your application. Wherever you render a `<Link>`, an anchor (`<a>`) will be rendered in your HTML document. This component behaves like an ordinary <a>, it will redirect you to the route that's defined as a prop. Let's see the example below. We we can pass other parameters to make the url to behave dynamically, we'll see it later on this day.

````javascript
import {BrowserRouter as Router, Switch,Route, Link} from "react-router-dom";
import {About, Users, Home} from 'pages'

function NavBar(){
    return(
        <nav>
          <ul>
            <li>
              <Link to="/">Home</Link>
            </li>
            <li>
              <Link to="/about">About</Link>
            </li>
            <li>
              <Link to="/users">Users</Link>
            </li>
          </ul>
        </nav>
    )
}
export default App(){
        return(
           <Router> 
                <NavBar/>
                    <Switch>
                        <Route path="/about">
                            <About />
                        </Route>
                        <Route path="/users">
                            <Users />
                        </Route>
                        <Route path="/">
                            <Home />
                        </Route>
                    </Switch>
                <Footer/>
            <Router/>
        )
    }

````
## Error Pages

Sometimes there is a chance that the user types an url that does not correspond to a `<Route>` element defined within our `<Switch/>` or maybe some error happened and the route does not exist. As a developers, we must handle this situation. The most common solution to this problem is to create a `<NotFound>` page and implement the component as it shows below

````javascript
import {BrowserRouter as Router, Switch,Route, Link} from "react-router-dom";
import {About, Users, Home} from 'pages'

function NotFound(){
    return(
        <div>
            <h1>Ooops! We couldn't find the page that you are looking for</h1>
        </div>
    )
}
export default App(){
        return(
           <Router> 
                <NavBar/>
                    <Switch>
                        <Route path="/about">
                            <About />
                        </Route>
                        <Route path="/users">
                            <Users />
                        </Route>
                        <Route path="/">
                            <Home />
                        </Route>
                         <Route path="*">
                            <NotFound />
                         </Route>
                    </Switch>
                <Footer/>
            <Router/>
        )
    }

````
By adding an `*` to the prop `path` we tell the `<Route>` component that every URL that the browser shows it's a match, but we don't want to show this error page every time, that's why we add it below the other `<Route>` elements. If we don't have any route match, it will show us the `<NotFound>` page. 

## Dynamic Routing
