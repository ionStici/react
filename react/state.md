# The useState Hooks

React Hooks are functions that let us manage the internal state of components and handle post-rendering side effects directly form our functional components.

<br>

## Update Function Component State

The _State Hook_ is a named export from the React library.

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

As argument to `useState()` we can pass an _initial value_ that will be used during the first render. Use `null` if you don't have an initial value.

By updating the state using the state setter, **we trigger a component re-render**.

- In React, we don't do direct DOM manipulations.
- A view is updated by re-rendering an entire component responsible for that view.
- A component is re-rendered when its state is updated.

**State is preserved throughout re-renders**, until the component is unmounted.

_p.s._ We can make as many calls to `useState()` as we want. It's recommended to split the state into multiple sections, each responsible for its part of the application.

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

When updating an array in state, we do not just add new data to the previous array, instead we replace the previous array with a brand new array. This means that any information that we want to save from the previous array needs to be explicitly copied over to our new array.

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

When the state updates, it triggers a re-render of the component using the new state data, including child components that receive that data as a prop.

React updates the actual DOM only where necessary. This means you don't have to worry about changing the DOM. **You simply declare what the UI should look like.**

`state` is completely encapsulated to its component (unless you pass `state` data to a child component as `props`).

Use state when you need to change something on the screen that a particular component is responsible for.

<br>

## State vs. Props

### State

- **Internal** data, owned by component

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
