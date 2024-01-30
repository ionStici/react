## Programmatic Navigation with useNavigate

<!-- <Route index element={<Navigate replace to="cities" />} /> -->

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
