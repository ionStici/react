# The useState Hook

## Table of Contents

- [Using the State Hook](#using-the-state-hook)
- [Initializing State With a Callback](#initializing-state-with-a-callback)
- [Previous State](#previous-state)
- [Arrays and Objects in State](#arrays-and-objects-in-state)
- [State vs. Props](#state-vs-props)
- [State Management](#state-management)
- [Stateless Components Inherit from Stateful Components](#stateless-components-inherit-from-stateful-components)
- [Child Components Update Their Parents' state](#child-components-update-their-parents-state)
- [Child Components Update Sibling Components](#child-components-update-sibling-components)
- [Component Categories](#component-categories)

## Using the State Hook

The `useState` Hook is a named export from the React library.

```jsx
import React, { useState } from "react";

function App() {
  const [count, setCount] = useState(0);

  const handleClick = () => setCount(1);

  return <button onClick={handleClick}>Number: {count}</button>;
}
```

`useState()` is a function that returns an array with 2 values that we destructure.

Follow this naming pattern: `[toggle, setToggle]`

1. _current state_ - the current value of this state.
2. _state setter_ - a function that we can use to update the value of this state.

As argument to `useState()` we can pass an _initial value_ that will be used during the initial render. Use `null` if you don't need an initial value.

By updating the state using the state setter, **we trigger a component re-render**.

- In React, we don't do direct DOM manipulations.
- A view is updated by re-rendering an entire component responsible for that view.
- A component is re-rendered when its state is updated.

**State is preserved throughout re-renders**, until the component is unmounted.

_p.s._ We can make as many calls to `useState()` as we want. It's recommended to split the state into multiple sections, each responsible for its part of the application.

## Initializing State With a Callback

_Lazy Initial State_

```jsx
useState(() => {
  const data = localStorage.getItem("data");
  if (!data) return 0;
  return JSON.parse(data);
});
```

When the initial value of the `useState` hook depends on some sort of computation, we can pass in a callback function that React will execute (only) on its initial render.

## Previous State

```jsx
function App() {
  const [count, setCount] = useState(1);
  const increment = () => setCount((prev) => prev + 1);

  return <button onClick={increment}>Increment: {count}</button>;
}
```

- The state setter callback function receives the previous state value as an argument.
- The value returned by this state setter callback will be used as the next state value.

This approach guarantees that we are working with the most current value of state.

## Arrays and Objects in State

```jsx
function App() {
  const [list, setList] = useState([1]);
  const [data, setData] = useState({ name: "John" });

  const handleClick = () => {
    setList((prev) => [...prev, 2]);
    setData((prev) => ({ ...prev, hobby: "Coding" }));
  };

  return <button onClick={handleClick}>Click</button>;
}
```

When updating the state, the state setter function replaces the previous value with a new value that it returns.

**This means that any data that we want to preserve from a previous array or object must be explicitly copied over to the new array or object.** _First we copy the values from the previous array or object by destructuring it, and only then we add new data._

## State vs. Props

React expects you to never modify the state directly, instead always use the state setter function.

- `props` for data that can be changed only by another component, `state` for data that the component itself can change.

- `props` for building re-usable components, `useState` for changing what we see on the screen dynamically.

When the state updates, it triggers a re-render of the component using the new state data, including child components that receive that data as a prop.

React updates the actual DOM only where necessary. This means you don't have to worry about changing the DOM. **You simply declare what the UI should look like.**

`state` is completely encapsulated to its component (unless you pass `state` data to a child component as `props`).

Use state when you need to change something on the screen that a particular component is responsible for.

### State

- **Internal** data, owned by a component
- Component "memory"
- Can be updated by the component itself
- Updating state causes component to re-render
- Used to make components interactive

### Props

- **External** data, owned by parent component
- Similar to function parameters
- Read-only
- **Receiving new props causes component to re-render.** Usually when the parent's state has been updated
- Used by parent to configure child component ("settings")

## State Management

**Local State:** State needed only by one or few components. State that is defined in a component and only that component and child components have access to it (by passing via props).

**Global State:** State that many components might need. Shared state that is accessible to every component in the entire application. Tools for managing global state: Context API & Redux.

**Derived State:** state that is computed from an existing piece of state or from props.

## Stateless Components Inherit from Stateful Components

- “Stateful” describes any component that has a state property;
- “Stateless” describes any component that does not.

A child component can access the state of its parent component through props.

```jsx
function Parent() {
  const [name, setName] = useState("John");
  return <Child name={name} />;
}

function Child({ name }) {
  return <h1>{name}</h1>;
}
```

- `props` for storing information that can be changed only by a different component.
- `state` for storing information that the component itself can change.

## Child Components Update Their Parents' state

```jsx
function Parent() {
  const [toggle, setToggle] = useState(true);
  const changeState = () => setToggle((prev) => !prev);

  return <Child toggle={toggle} onClick={changeState} />;
}

function Child({ toggle, onClick }) {
  return <button onClick={onClick}>{toggle + ""}</button>;
}
```

## Child Components Update Sibling Components

**Lifting State Up:** whenever multiple sibling components need access to the same state, we move that piece of state up to the first common parent component.

_Concept:_ one stateless component displays information, and a different stateless component offer the ability to change that information.

```jsx
const ChildOne = (props) => <button onClick={props.onClick}>Click</button>;
const ChildTwo = (props) => <p>{props.text}</p>;

function Parent() {
  const [text, setText] = useState("Hello");
  const changeState = () => setText("Hello World");

  return (
    <>
      <ChildOne onClick={changeState} />
      <childTwo text={text} />
    </>
  );
}
```

A child component updates its parent’s state, and the parent passes that state to a sibling component.

## Component Categories

- **Stateless / Presentational Components**

  - No state
  - Receive props
  - Simply present data

- **Stateful Components**

  - Have state

- **Structural Components**

  - Pages, layouts of the app, provides structure
  - Result of component composition
