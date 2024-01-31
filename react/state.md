# The useState Hooks

## Table of Content

- [Update Function Component State](#update-function-component-state)
- [Initializing State With a Callback](#initializing-state-with-a-callback)
- [Previous State](#previous-state)
- [Arrays in State](#arrays-in-state)
- [Objects in State](#objects-in-state)
- [Notes](#notes)
- [State vs. Props](#state-vs-props)
- [State Management](#state-management)
- [Stateless Components Inherit from Stateful Components](#stateless-components-inherit-from-stateful-components)
- [Child Components Update Their Parents' state](#child-components-update-their-parents-state)
- [Child Components Update Their Sibling Components](#child-components-update-their-sibling-components)
- [Component Categories](#component-categories)

<br>

## Update Function Component State

The `useState` Hook is a named export from the React library.

```jsx
import React, { useState } from "react";

function App() {
  const [count, setCount] = useState(1);

  const handleClick = () => {
    setCount(count + 1);
  };

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

<br>

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

<br>

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

<br>

## Arrays in State

JavaScript arrays are the best data model for managing and rendering JSX lists.

```jsx
const arr = [1, 3, 5, 7];

function App() {
  const [nums, setNums] = useState([9, 12]);

  const handleClick = () => useState((prev) => [...arr, ...prev]);

  return <button onClick={handleClick}>{nums.join(" ")}</button>;
}
```

We define an `arr` _static data model_ outside of our function component since it doesn't need to be recreated each time our component re-renders.

Then, the `nums` array contains _dynamic data_, meaning that it changes.

When updating an array in state, we do not just add new data to the previous array, instead we replace the previous array with a brand new array. **This means that any data that we want to save from the previous array needs to be explicitly copied over to our new array.**

<br>

## Objects in State

```jsx
function App() {
  const [pair, setPair] = useState({});

  const handleClick = (name, value) => {
    setPair((prev) => ({ ...prev, name: value }));
  };
}
```

When updating the object with new data, first we copy the values from the previous object and only then we set more values, the same technique as when working with arrays.

<br>

## Notes

React expects you to never modify the state directly, instead always use the state setter.

A React App is basically just a lot of components, setting state and passing props to one another.

`props` for data that can be changed only by another component. `state` for data that the component itself can change.

`props` for building re-usable components. `useState` for changing what we see on the screen dynamically.

When the state updates, it triggers a re-render of the component using the new state data, including child components that receive that data as a prop.

React updates the actual DOM only where necessary. This means you don't have to worry about changing the DOM. **You simply declare what the UI should look like.**

`state` is completely encapsulated to its component (unless you pass `state` data to a child component as `props`).

Use state when you need to change something on the screen that a particular component is responsible for.

<br>

## State vs. Props

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

<br>

## State Management

**Local State:** State needed only by one or few components. State that is defined in a component and only that component and child components have access to it (by passing via props).

**Global State:** State that many components might need. Shared state that is accessible to every component in the entire application. Tools for managing global state: Context API & Redux.

**Derived State:** state that is computed from an existing piece of state or from props.

<br>

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

<br>

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

<br>

## Child Components Update Their Sibling Components

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

<br>

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

### Component Composition

**Prop Drilling** occurs when a parent component passes data down to its children and then those children pass the same data down to their own children.

**Component Composition** - technique of combining different components using the `children` prop (or explicitly defined props) / for reusable and flexible components / to fix a prop drilling problem / great for creating layouts.

```jsx
// Component Composition Example
function App() {
  return (
    <Layout>
      <NavBar>
        <Logo />
      </NavBar>

      <List>
        <Box />
        <Box />
      </List>
    </Layout>
  );
}
```
