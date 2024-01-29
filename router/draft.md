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

## Storing State in the URL

The URL is an excellent place to store UI state and an alternative to `useState` in some situations. Examples: open/closed panels, currently selected list item, list sorting order, applied list filters.

1. Easy way to store state in a **global place**, accessible to **all components** in the app
2. Good way to **"pass" data** from one page into the next page
3. Makes it possible to **bookmark and share** the page with the exact UI state it had at the time

## Programmatic Navigation with useNavigate

<!-- <Route index element={<Navigate replace to="cities" />} /> -->

<br>
