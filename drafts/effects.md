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

## useEffect with Async

```jsx
useEffect(() => {
  async function fetchData() {
    const res = await fetch();
    const data = await res.json();
  }
  fetchData();
}, []);
```

When react's strict mode is activated in React 18, our effects will run twice, but only in development phase, that's why we usually get 2 console logs.

<br>

## The useEffect Dependency Array

`useEffect` is like an event listener that is listening for one dependency to change.

Whenever a dependency changes, it will execute the effect again.

`useEffect` is a synchronization mechanism, a mechanism to synchronize effects with the state of the application.

Whenever a dependency changes, the effect is executed again. Dependencies are always state or props. And when state or props changes, the component will re-render, this means that effects and the life cycle of a component instance are deeply interconnected.

We can use the dependency array to run effects when the component renders and re-renders.

The `useEffect` hook is about synchronization and the component lifecycle.

```jsx
useEffect(runEffect);
useEffect(runEffect, []);
useEffect(runEffect, [x, y, z]);
```

- **When we have multiple dependencies - `[x, y, z]` -** it means that the effect synchronizes with these dependencies, in terms of lifecycle it means that the effect will run on the initial render and also on each re-render triggered by updating one of the dependencies. The effect will be executed each time the component instance is being re-rendered by an update to `[x, y, z]`, but if other state or prop is updated then this particular effect will not be executed.

- **An empty dependency array**: the effect synchronizes with no state or props, runs only on mount (initial render).

- **No array at all**: effect synchronizes with everything, the effect will run on every render (usually bad).

### When are effects executed?

The process start with mounding (initial render) the component instance. Then, the result of rendering is committed to the dom, and then dom changes are painted onto the screen by the browser.

Effects are only executed after the browser has painted the component instance on the screen, and not immediately after render. Why? Because effects may contain long-running processes such as fetching data.

One important consequence of the fact that effects do not run during render is that if an effect sets state, then a second additional render will be required to display the UI correctly, because of this you should bot overuse effects. In other words: if an effect sets state, an **additional render** will be required.

<br>
