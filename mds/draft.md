# Advanced React Patterns

## Table of Contents

- [Render Props Pattern](#render-props-pattern)
- [Higher-Order Component Pattern (HOC)](#higher-order-component-pattern-hoc)

## Render Props Pattern

The **Render Props Pattern** is a technique where a prop named `render` is passed to a component. This prop is a function that tells the component how it should render its content.

This pattern is particularly useful when direct insertion of JSX via the `children` prop is inadequate because the component requires specific instructions on how to render its content.

```jsx
function App() {
  return <List items={apiData} render={item => <li>{item}</li>} />;
}
```

```jsx
function List({ items, render }) {
  return <ul>{items.map(render)}</ul>;
}
```

In this setup, the `List` component accepts a `render` prop which is a function that tells the component what and how to render. This means that the parent component, and not the `List` itself, gets to decide how the items are rendered.

P.S. It is worth noting that nowadays, custom React hooks are often preferred for sharing logic between components, as they provide a more straightforward and cleaner approach compared to the render props pattern.

## Higher-Order Component Pattern (HOC)

A **Higher-Order Component (HOC)** is a component that takes in another component and then returns a new component that is an enhanced version of the initial component.

```jsx
// A simple component that we want to enhance
function Display({ message }) {
  return <div>{message}</div>;
}

// Higher-Order Component (HOC)
function withEnhancement(WrappedComponent) {
  const message = 'Hello from HOC!';
  return <WrappedComponent {...props} message={message} />;
}

// Create HOC
const EnhancedDisplay = withEnhancement(Display);

// Use the HOC
function App() {
  return <EnhancedDisplay />;
}
```

An HOC is a pure function with zero side-effects. It doesn't modify the original component, but rather creates a wrapper component that contains the original component.
