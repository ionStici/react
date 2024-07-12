# Navigate

## The `Navigate` Component

```jsx
import { Navigate } from 'react-router-dom';

// Navigate inside Route for URL redirecting
<Route index element={<Navigate replace to="cities" />} />;
```

The `Navigate` component is used to declaratively redirect users to another route. t's typically used in the rendering logic of a component.

A common case for redirecting a user: a user wants to access a `/profile` page that requires authentication but is not currently signed in.

```jsx
function UserProfile({ loggedIn }) {
  if (!loggedIn) return <Navigate to="/login" />;

  return <p>User profile content</p>;
}
```

If the `Navigate` component is rendered, the user will automatically be taken to the location specified by the `to` prop.

**Use Cases:** Commonly used for redirecting a user after an action, like a successful form submission or if the user lands on a page they shouldn't access (e.g., a restricted page without the necessary permissions).

## The `useNavigate` Hook

React Router provides a mechanism for updating the browser's location imperatively using the `useNavigate` hook. The `useNavigate` hook returns a `navigate` function that allows us to specify a path where we'd like to navigate.

**Key Points:**

- **Imperative Navigation:** It's a function that you can call to change the route.
- **Flexibility:** It can be used anywhere in your component's logic, not just in the rendering part.
- **Use Cases:** Useful for navigating after certain actions like form submissions, button clicks, or other interactive elements.

```js
import { useNavigate } from 'react-router-dom';

function Comp() {
  const navigate = useNavigate();

  const goHomepage = () => navigate('/');
  const goProfile = () => navigate('/profile');
  const goBack = () => navigate(-1);
  const goForward = () => navigate(1);
}
```

The `useNavigate` function also gives us the ability to programmatically navigate our users through their [history](https://developer.mozilla.org/en-US/docs/Web/API/Window/history) stack.

Scenarios like enabling users to go forward or backward in an application, or redirecting users to their previous page after they’ve logged in, are great use cases for this functionality.

To navigate a user through their history stack using `useNavigate()`, we’d pass in an integer to indicate where in the history we’d like to travel. A positive integer navigates forward and a negative integer navigates backward.

`-1` or `1` will navigate to the previous or to the next URL in the history stack. Other integers like for example `3`, will navigate 3 URLs forward in the history stack.
