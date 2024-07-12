# React Routes

[React Router Official Website](https://reactrouter.com/)

```jsx
npm i react-router-dom
```

## Creating Routes

```jsx
import { BrowserRouter, Route, Routes } from 'react-router-dom';

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<Homepage />} />
        <Route path="*" element={<PageNotFound />} />
        <Route path="contact" element={<Contact />} />
        <Route path="login" element={<Login />} />
      </Routes>
    </BrowserRouter>
  );
}
```

- Define routes using the `<Route />` component
- Using the `path` and `element` props inside the `<Route />` component, we indicate which component to render at which URL path.
- If the user accesses a URL path that does not exist, we can render a default component using the `path="*"` prop

## Nested Routes

When nesting routes, the child route `path` is relative to the parent route `path`.

```jsx
<Routes>
  <Route path="about" element={<About />}>
    <Route path="team" element={<Team />} />
  </Route>
</Routes>
```

When the url matches `/about`, the `<About />` component will render, and if it matches `/about/team` then the `<Team />` component will render in addition to `About`.

```jsx
import { Outlet } from 'react-router-dom';

function About() {
  return <Outlet />;
}
```

We have to indicate explicitly where to render the `<Team />` component within `<About />` parent component. For this, we use the `<Outlet />` component inside the parent `<About />` component.

When we visit `/about/team`, the router will render `<About />` and its child route component `<Team />` wherever the `<Outlet />` component is defined. The router will replace `<Outlet />` with our defined child route.

## Index Routes

```jsx
<Route path="about" element={<About />}>
  <Route index element={<Info />} />
  <Route path="team" element={<Team />} />
</Route>
```

A nested `index` route will render at `<Outlet />` when it's parent route's `path` matches.
