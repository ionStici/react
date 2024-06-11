# Advanced React Patterns

## Table of Contents

- [The Compound Component Pattern](#the-compound-component-pattern)
- [Render Props Pattern](#render-props-pattern)
- [Higher-Order Component Pattern (HOC)](#higher-order-component-pattern-hoc)

## The Compound Component Pattern

**Compound Component:** a set of related components that together achieve a common and useful task. Create a parent component, and then a few different child components that really belong to the parent, and that really only make sense when used together with the parent component.

```jsx
// Counter.jsx
import React, { useState, createContext, useContext } from 'react';

// 1. Create a Context
const CounterContext = createContext();

// 2. Create Parent Component
function Counter({ children }) {
  const [count, setCount] = useState(0);
  const increase = () => setCount(c => c + 1);
  const decrease = () => setCount(c => c - 1);

  return (
    <CounterContext.Provider value={{ count, increase, decrease }}>
      <div>{children}</div>
    </CounterContext.Provider>
  );
}

// 3. Create child components to help implementing the common task
function Count() {
  const { count } = useContext(CounterContext);
  return <span>{count}</span>;
}

function Label({ children }) {
  return <span>{children}</span>;
}

function Increase({ icon }) {
  const { increase } = useContext(CounterContext);
  return <button onClick={increase}>{icon}</button>;
}

function Decrease({ icon }) {
  const { decrease } = useContext(CounterContext);
  return <button onClick={decrease}>{icon}</button>;
}

// 4. Add child components as properties to parent component
Counter.Count = Count;
Counter.Label = Label;
Counter.Increase = Increase;
Counter.Decrease = Decrease;
// In JS we can add properties almost to everything including functions, because functions are actually objects.

// Alternatively, we can export each child component
// export { Count, Label, Increase, Decrease };

export default Counter;
```

```jsx
// App.jsx
import Counter from './Counter';

export default function App() {
  return (
    <Counter>
      <Counter.Label>Counter</Counter.Label>
      <Counter.Increase icon="+" />
      <Counter.Count />
      <Counter.Decrease icon="-" />
    </Counter>
  );
}
```

#### Key Concepts

1. **Parent Component:** The main component that holds the state and provides the context for its child components.
2. **Child Components:** Smaller components that are used to build up the structure of the parent component. These components communicate with the parent component to share state and behavior.

#### Steps to Implement

1. **Create the Parent Component:** This component will manage the state and provide any necessary context to its child components using React’s `Context` API.
2. **Create the Child Components:** These components will use the context provided by the parent component to access the shared state and behavior.
3. **Provide Context:** The parent component should provide context values to its child components, allowing them to communicate and share state.

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
