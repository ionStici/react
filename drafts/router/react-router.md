# React Router

## Table of Content

- [What is Routing?](./routing.md)
- [Providing a Router](#providing-a-router)
- [Linking to Routes](#linking-to-routes)
- [Dynamic Routes](#dynamic-routes)
- [useParams Hook](#useparams-hook)
- [Nested Routes](#nested-routes)
- [Navigate Component](#navigate-component)
- [useNavigate](#usenavigate)

<br>

## Providing a Router

- Install React Router v6 `npm install --save react-router-dom@6`

In the React Router paradigm, the different views of the application (called routes) are just React components that we render.

```js
import {
  Outlet,
  Route,
  RouterProvider,
  createBrowserRouter,
  createRoutesFromElements,
} from "react-router-dom";
```

```js
function Root() {
  return (
    <>
      <Header />
      <main>
        <Outlet />
      </main>
      <Footer />
    </>
  );
}
```

```js
const router = createBrowserRouter(
  createRoutesFromElements(
    <Route path="/" element={<Root />}>
      <Route path="about" element={<About />} />
      <Route path="articles" element={<Articles />} />
      <Route path="profile" element={<Profile />} />
    </Route>
  )
);

function App() {
  return <RouterProvider router={router} />;
}
```

- `createBrowserRouter` will define a router that prevents URL changes from causing the page to reload. Instead, URL changes will allow the `router` to determine which of its defined routes to render while passing along information about the current URL's path as props.

- We then store that in a `router` variable and make it available application-wide by providing it to the root of our application using `RouterProvider`.

- Here `createBrowserRouter()` we define the routes using JSX or objects. This method accepts an array of `<Route />` objects. To configure the routes with JSX, we need the `createRoutesFromElements` method.

- For each route (view), various URL paths is defined using the `<Route />` component. The `<Route>` component is designed to render (or not render) a component based on the current URL path. Each `<Route>` component should include:
  - **`path` prop** - indicates the exact URL path that will cause the route to render.
  - **`element` prop** - indicates the component that will be rendered.

<div></div>

- _Render Components with every page view:_ define a root-level component **`<Root/>`** that contains layout elements we want to render consistently and nest the rest of the routes within this root-level component.

<br>

## Linking to Routes

```js
import { Link, NavLink } from "react-router-dom";
<Link to="/about">About</Link>
<NavLink to="/about">About</NavLink>
```

- `Link` and `NavLink` components work like anchor tags.
- The `to` prop indicates the location to redirect the user to.

Both links will generate anchor tags displaying the given text and will take the user to the given path indicated within `to` prop when clicked.

The default behavior of an anchor tag (refreshing when the page loads) will be disabled.

If we provide a `/` forward slash, the path will be treated as an absolute path. React Router will assume this path is navigating from the root directory.

Unlike the `Link` component, `NavLink` will automatically get an `'active'` class applied to it. We can pass a function to `className` or `style` to further customize the styling of an active or inactive `NavLink`:

```js
<NavLink
  to="about"
  className={({ isActive }) => (isActive ? "activeLink" : "inactiveLink")}
>
  About
</NavLink>
```

<br>

## Dynamic Routes

**Static Routes** - match a distinct and unique path.

**Dynamic Routes** - a single route that match any path with the pattern: `/articles/ + title`.

We can accomplish dynamic routes and render specific components by using React Router URL parameters.

URL parameters are dynamic segments of a URL that act as placeholders for more specific resources.

They appear in a dynamic route as a colon `:` followed by a variable name.

```js
const route = createBrowserRouter(createRoutesFromElement(
    <Route path="articles" element={<Articles />} />
    <Route path="articles/:title" element={<Article />} />
    <Route path="authors/:name" element={<Author />} />
));
```

- the `path` prop `'articles/:title'` contains the URL parameter `:title`
- This means that when the user navigates to pages such as `'/articles/whatever'`, the `<Article />` component will render
- When the `Article` component is rendered in this way, it can access the actual value of that `:title` URL parameter to determine which article to display. A single route can even have multiple parameters `'articles/:title/comments/:commentId'`

<br>

## useParams Hook

Using Dynamic Routes and URL parameters - determine what components to display.

React Router provides a hook `useParams()` for accessing the value of URL parameters. When called, `useParams()` returns an object that maps the names of URL parameters to their value in the current URL.

```js
import { Link, useParams } from "react-router-dom";

function Article() {
  let { title } = useParams();
  return <h1>{title}</h1>;
}
```

- Inside the `Article` component, `useParams` is called which returns an object.
- We destructure the object to extract the value of the URL parameter from that object, storing it in a variable named `title`.
- We then use the `title` value for rendering based on some logic.

<br>

## Nested Routes

A **nested route** is a `Route` within a `Route`. When nesting routes, the child route `path` is relative to the parent route `path`. So there is no need to include the parent `path` in the child `path` prop.

```js
<Route path="/about" element={<About />}>
  <Route path="me" element={<Me />} />
</Route>
```

Using this nested route, the `About` component will render when the path starts with `/about`. If the path matches `/about/me`, the `Me` component will render in addition to the `About` component.

We have to indicate explicitly where to render the `Me` child component within `About` parent component. For this, we need the React Router `Outlet` component inside the parent `About` component.

```js
import { Outlet } from "react-router-dom";

function About() {
  return (
    <>
      <h1>About</h1>
      <Outlet />
    </>
  );
}
```

Now when we visit `/about/me`, the router will render `About` and its child route component `Me` wherever the `Outlet` component is defined. The router will replace `Outlet` with our defined child route.

### Index Routes

An index route will render on its parent's `path`. It uses the `index` prop instead of a `path` prop.

```js
<Route path="/about" element={<About />}>
  <Route index element={<Index />} />
  <Route path="me" element={<Me />} />
</Route>
```

An index route is like a **default** `Route` that will render in its parent's `Outlet` when the path matches the parent `path` exactly.

<br>

## Navigate Component

How to redirect declaratively by rendering a `Navigate` component that updates the browser’s current location.

A common case for redirecting a user: a user wants to access a `/profile` page that requires authentication but is not currently signed in.

```js
import { Navigate } from "react-router-dom";

const UserProfile = ({ loggedIn }) => {
  if (!loggedIn) {
    return <Navigate to="/" />;
  }

  return (
    // user profile content
  )
};
```

If the `Navigate` component is rendered, the user will automatically be taken to the location specified by the `to` prop.

<br>

## useNavigate

React Router provides a mechanism for updating the browser's location imperatively using the `useNavigate` hook.

The `useNavigate` function returns a `navigate` function that allows us to specify a path where we'd like to navigate.

```js
import { useNavigate } from "react-router-dom";

function Comp() {
  const navigate = useNavigate();

  const goHomePage = () => navigate("/");
  const goProfile = () => navigate("/profile");
  const goBack = () => navigate(-1);
  const goForward = () => navigate(1);
}
```

The `useNavigate` function also gives us the ability to programmatically navigate our users through their [history](https://developer.mozilla.org/en-US/docs/Web/API/Window/history) stack.

Scenarios like enabling users to go forward or backward in an application, or redirecting users to their previous page after they’ve logged in, are great use cases for this functionality.

To navigate a user through their history stack using `useNavigate()`, we’d pass in an integer to indicate where in the history we’d like to travel. A positive integer navigates forward and a negative integer navigates backward.

`-1` or `1` will navigate to the previous or to the next URL in the history stack. Other integers like for example `3`, will navigate 3 URLs forward in the history stack.

<br>

<!-- Query Parameters / useSearchParams -->
<!-- errorElement -->
