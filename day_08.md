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

Let's assume that we have a typical e-commerce with over 1000 products, we're gonna have to create a link for each product? Nope, that's where Dynamic Routing comes to save our time by making this task really easy. 

First of All, we're gonna need to define our routes to handle dynamic routing. Let's get back to the e-commerce example and mix it with the example that's on top.

````javascript
import {BrowserRouter as Router, Switch,Route, Link} from "react-router-dom";
import {About, Users, Home, Product} from 'pages'

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
                         <Route path="/products/:id">
                            <Product />
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
Below the path `/` we added another route called `/products/:id` here, by defining an `:id` we're telling React Router that this route is dynamic, and the attribute that comes after the `/products/` can be picked up from another function that react router provides us (we'll jump on that later on). 

````javascript
import { Link, useParams } from "react-router-dom";


export default ProductCard({ title, id, price }){
        return(
           <div>
                <h1>{title}</h1>
                <h3>${price}</h3>
                </Link to=`/products/${id}` >
           </div>
        )
    }

````

On a typical e-commerce we'll have a card that shows the main product specifications (the product name and his respective price in this case) and it will redirect us to a bigger product page with other specifications and a bigger description. This card takes as props the `title`, `path` and `price` of the product. What we need to do here is to redirect the user to the specific product page that will show him more info about it. 

````javascript
import { useParams } from "react-router-dom";

export default ProductPage(){
    const {id} = useParams()

    const [product, setProduct] = useState()

    useEffect(() => {
        fetch(`/products/${id}`)
        .then(product => setProduct(product))
    }, [])

        return(
           <div>
                <h1>{product.title}</h1>
                <h3>${product.description}</h3>
                <img>${product.price}</h3>
                <button>Add to cart</button>
           </div>
        )
    }

````
The `useParams` hook returns an object of key/value pairs of the dynamic params from the current URL that were matched by the `<Route path>` thanks to the `useParams` hook that React Router provides us, we can catch the parameter `id` that comes within the product URL, and with this id we can make a simple API call to get data to our component with a little bit of help from the `useState` and `useEffect` hooks also.


## Protected Routes

Sometimes our application will have certain pages that are not allowed for everyone to access. This is where we need to implement `ProtectedRoutes` and `PublicRoutes` to our project. For implement them we need to go back to where we defined our routes. In this example it will be App.jsx

`````javascript

    export default App(){

        const routes = [{
            path: "/about"
            element: <About/>,
            private: false
        },{
            path: "/users"
            element: <Users/>,
            private: true
        },{
            path: "/products/:id"
            element: <Product/>,
            private: false
        },{
            path: "/"
            element: <Home/>,
            private: false
        },{
            path: "*"
            element: <NotFound/>,
            private: false
        }]
        return(
           <Router> 
                <NavBar/>
                    <Switch>
                       {
                        routes.map(route =>{
                            return(
                                <Route path={route.path}>
                                    {route.private ? <PrivateRoute element={route.element}/> : <PublicRoute element={route.element}/>}
                                </Route>
                            )
                        })
                       }
                    </Switch>
                </Footer>
            </Router>
        )
    }

````

`````
