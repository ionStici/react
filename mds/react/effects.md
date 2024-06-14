# Effects and Data fetching

## Table of Contents

- [The Component Instance Lifecycle](#the-component-instance-lifecycle)
- [What are Side Effects](#what-are-side-effects)
- [The useEffect Hook](#the-useeffect-hook)
- [The Dependency Array](#the-dependency-array)
- [The Cleanup Function](#the-cleanup-function)

## The Component Instance Lifecycle

The lifecycle of a component instance encompasses 3 phases:

1. **Mount / Initial Render**

2. **Re-render** - Happens when: state change, prop change, parent re-render, context change.

3. **Unmount** - the component instance is destroyed.

We can execute code in these different phases of a component's lifecycle using the `useEffect` hook.

## What are Side Effects

A **side effect** is any interaction between a react component and the world outside that component. Examples: data fetching, timers, accessing the DOM, etc.

**Important:** Side effects should not be defined in the render logic.

We can create side effects in 2 different places in React: inside **Event Handlers** and using the **`useEffect` hook**.

## The useEffect Hook

```jsx
import { useState, useEffect } from 'react';

function App() {
  const [data, setData] = useState(null);

  useEffect(() => {
    async function fetchData() {
      const res = await fetch();
      const data = await res.json();
      setState(data);
    }

    fetchData();

    return () => console.log('Cleanup');
  }, []);

  return <p>JSX</p>;
}
```

- Setting state in the render logic will cause the component to re-render itself again and again indefinitely. The solution to this is to set state inside the `useEffect` hook.

- The `useEffect` hook is composed of 3 parts: the effect code, the dependency array, a cleanup function that must be returned.

- The first argument of `useEffect` is a callback (nicknamed "effect") that should contain the side effect code.

- The second argument of `useEffect` is the dependency array.

- Optionally, `useEffect` can return a cleanup function that will be called **before** the component re-renders or unmounts.

- Effects are executed after the component mounts (initial render), and after subsequent re-renders (according to the dependency array).

## The Dependency Array

`useEffect` is like an event listener that is listening for its dependencies to change. Whenever a dependency changes, the effect is executed again.

Dependencies are always state or props, and when state or props changes the component re-renders, this means that effects and the lifecycle of components are interconnected.

```jsx
useEffect(runEffect);
useEffect(runEffect, []);
useEffect(runEffect, [x, y, z]);
```

_Different Scenarios:_

- **Multiple dependencies:** the effect runs on the initial render and on each re-render triggered by updating one of the dependencies, it synchronizes with these dependencies. If other state or prop is updated then this particular effect will not be executed.

- **An empty dependency array:** the effect runs only on mount, no synchronization.

- **No array at all:** the effect runs on every render, it synchronizes with everything.

### When are effects executed?

The component instance mounts, the result of rendering is committed to the DOM, the DOM changes are painted onto the screen.

Effects are only executed after the browser has painted the component instance on the screen, and not immediately after render. Why? Because effects may contain long-running processes such as fetching data.

This means that if an effect sets state, then a second additional render will be required to display the UI correctly (don't overuse effects).

## The Cleanup Function

```jsx
useEffect(() => {
  // code ...

  return () => '';
}, []);
```

_The cleanup function runs on two different occasions:_

1. Before the effect is **executed again** (in case we need to cleanup the result of the previous side effect)
2. After a component has **unmounted** (in case we need to reset the side effect created)

When we need a cleanup function (which is optional)? Whenever the side effect should be removed. Examples: start timer -> stop timer, add event listener -> remove listener, etc.

Each effect should do only one thing, use one `useEffect` hook for each side effect, this makes effects easier to clean up.

The cleanup function runs after the component has already unmounted, and even so, thanks to JavaScript closures the cleanup function will still get access to the previous variable environment.

### Cleaning Up Data Fetching

_Race Condition_

```jsx
useEffect(() => {
  const controller = new AbortController();

  async function fetchData() {
    const res = await fetch('apilink', { signal: controller.signal });
  }

  fetchData();

  return () => controller.abort();
}, [dep]);
```

### Listening to a Keypress

```jsx
useEffect(() => {
  const callback = () => '';

  document.addEventListener('keypress', callback);

  return () => document.removeEventListener('keypress', callback);
}, []);
```

Keypress event listeners should be placed inside the `useEffect` hook, because they are side effects.

Each time the same component mounts, a new event listener is added to the document, always an additional one to the ones that we already have, so we should cleanup these event listeners.
