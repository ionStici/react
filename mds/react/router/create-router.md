# createBrowserRouter

`createBrowserRouter` is used to define the routes and the routing configuration for a React application.

It takes an array of route objects, each route object specifies the path, the component to render, and optionally loaders, actions, and error elements.

```jsx
// App.jsx
import { RouterProvider, createBrowserRouter } from 'react-router-dom';
import User, { loader as userLoader } from 'User';
import UserUpdate, { action as userUpdateDataAction } from 'UserUpdate';

const router = createBrowserRouter([
  {
    element: <Layout />,
    errorElement: <Error />,
    children: [
      {
        path: '/',
        element: <Home />,
      },
      {
        path: '/user',
        element: <User />,
        loader: userLoader,
        errorElement: <Error />,
      },
      {
        path: '/user/update',
        element: <UserUpdate />,
        action: userUpdateDataAction,
      },
    ],
  },
]);

function App() {
  return <RouterProvider router={router} />;
}
```

## loader

A route's **loader function** will fetch data when a path matches, just before the route component is rendered, conveniently providing the necessary data ahead of time.

The `useLoaderData` react router hook provides the data returned by the loader function.

```jsx
// User.jsx
import { useLoaderData } from 'react-router-dom';
import { getUser } from 'userAPI';

function User() {
  const user = useLoaderData();

  return <UserProfile user={user} />;
}

export async function loader() {
  const user = await getUser();
  return user;
}

export default User;
```

## action

**Making a Request:** React Router supports form submissions through the `action` function, which handles form data processing and side effects (like updating a database or redirecting).

`useActionData` is a react router hook that retrieves data returned by the route’s action function after a form submission. It is used within a route component to access any data or errors returned by the action.

When a `<Form>` is submitted, a request is made to the async action function, this function receives the `request` object, then here we can handle the real request logic using the data receives.

```jsx
// UserUpdate.jsx
import { Form, redirect, useActionData } from 'react-router-dom';
import { updateUserData } from 'userAPI';

function UserUpdate() {
  const formErrors = useActionData();
  console.error(formErrors?.phone);

  return (
    <Form method="POST">
      <input type="text" name="username" />
      <button>Update</button>
    </Form>
  );
}

export async function action({ request }) {
  const formData = await request.formData();
  const data = Object.formEntries(formData);

  const errors = {};

  if (validate(data)) {
    errors.phone = 'Wrong phone format.';
  }

  if (Object.keys(errors).length > 0) return errors;

  const newData = await updateUserData(data);

  return redirect(`/${newData.username}`);
}

export default UserUpdate;
```

## Loading UI

How to display a page-level loading element when the app fetches data.

```jsx
// Layout.jsx
import { Outlet, useNavigation } from 'react-router-dom';

function Layout() {
  const navigation = useNavigation();
  const isLoading = navigation.state === 'loading';

  if (isLoading) return <p>Loading...</p>;

  return <Outlet />;
}
```

## useFetcher

Pre-fetch data from a specified endpoint (action) without directly navigating to that path.

```jsx
// About.jsx
import { useFetcher } from 'react-router-dom';
import { useEffect } from 'react';

function About() {
  const fetcher = useFetcher();

  useEffect(() => {
    fetcher.load('/user');
  }, [fetcher]);

  return null;
}
```

- The `useFetcher` hook is called to create a `fetcher` object. The `fetcher` object provides methods to perform fetch-like operations.
- `fetcher.load('/user')` initiates a `GET` request to the `/user` endpoint without accessing the `/user` path.
