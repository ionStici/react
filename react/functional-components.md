# React Functional Components

React apps are built by creating and combining multiple components.

React components are simply functions that fulfill 2 rules:

1. Must start with an uppercase character
2. Must return either JSX or `null`

```jsx
function App() {
  return <h1>Hello, World!</h1>;
}
```

Besides returning, we can have any JavaScript logic inside function components.

Separation of concerns: One component per file, which, by using JSX, combines HTML, CSS and JS in one single block of code.

<br>
