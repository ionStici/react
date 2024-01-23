# React Functional Components

React apps are built by creating and combining multiple components.

React components are simply functions that fulfill 2 rules:

1. Must start with an uppercase character
2. Must return either JSX or `null`

```jsx
function Greet() {
  return <h1>Hello, World!</h1>;
}

function App() {
  return <Greet />;
}
```

Component composition: in the `App` component, `<Greet />` is used to include the output of the `Greet` component.

Besides returning, we can have any JavaScript logic inside functional components.

Separation of concerns: One component per file, which by using JSX: combines HTML, CSS and JS in one single block of code.

<br>
