# createBrowserRouter

`createBrowserRouter` is used to define the routes and the routing configuration for a React application.

It takes an array of route objects, each route object specifies the path, the component to render, and optionally loaders, actions, error elements.

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

A route's loader function will fetch data

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

```jsx
// Layout.jsx

function Layout() {
  return null;
}
```
