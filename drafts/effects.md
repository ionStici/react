# Effects and Data fetching

## The Component Instance Lifecycle

The lifecycle of a component encompasses the different phases that a component instance can go through over time. Phases:

1. **Mount / Initial Render**

2. **Re-render** - Happens when: State changes, Props change, Parent re-renders, Context changes.

3. **Unmount** - the component instance is destroyed and removed along with its state and props.

   Later a new instance of the same component can be mounted later, but this specific instance that was unmounted is gone.

We can define code to be executed at different phases of a component's lifecycle by using the `useEffect` hook.

<br>

## The useEffect Hook

Setting state in the render logic will cause the component to re-render itself again and again indefinitely. The solution for this is to set state inside the `useEffect` hook.

The idea of the `useEffect` hook is to give us a place where we can safely write side effects like data fetching.

The side effects registered with the `useEffect` hook will only be executed after certain renders, for example only after the initial render.

```jsx
import { useState, useEffect } from "react";

function App() {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetch()
      .then((res) => res.json())
      .then((data) => setData(data));
  }, []);

  return <p>JSX</p>;
}
```

We pass a callback into the `useEffect` hook (called "effect") that should contain the code that we want to run as a side effect.

The second argument of the `useEffect` hook is a dependency array. If the dependency array is empty then the `useEffect` hook will only run on mount.

<br>

## Effects

A side effect is any interaction between a React component and the world outside that component. Examples: data fetching, timers, accessing the DOM, etc.

Side effects should not be in render logic. Effects allow us to write code that will run at different moments of the component lifecycle: mount, re-render, unmount.

_We can create side effects in 2 different places in React:_

1. **Event Handlers**

   - Executed when the corresponding event happens.

   - Used to react to an event that happened in the user interface.

   - Event handlers are the preferred way of creating side effects, whenever possible we should not overuse the `useEffect` hook.

2. **Effects `useEffect`**

   - The `useEffect` hook is composed of 3 parts: the effect code, the dependency array, a cleanup function that must be returned.

   - Executed after the component mounts (initial render), and after subsequent re-renders (according to dependency array).

   - The cleanup function will be called before the component re-renders or unmounts.
