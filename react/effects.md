# Effects and Data fetching

## Table of Content

- [The Component Instance Lifecycle](#the-component-instance-lifecycle)
- [Side Effects](#side-effects)
- [The useEffect Hook](#the-useeffect-hook)
- [The Dependency Array](#the-dependency-array)

<br>

## The Component Instance Lifecycle

The lifecycle of a component instance encompasses 3 phases:

1. **Mount / Initial Render**

2. **Re-render** - Happens when: state change, prop change, parent re-render, context change.

3. **Unmount** - the component instance is destroyed.

We can execute code in these different phases of a component's lifecycle using the `useEffect` hook.

<br>

## Side Effects

A **side effect** is any interaction between a react component and the world outside that component. Examples: data fetching, timers, accessing the DOM, etc.

**Note:** Side effects should not be defined in the render logic.

We can create side effects in 2 different places in React: inside **Event Handlers** and using the **`useEffect` hook**.

<br>

## The useEffect Hook

```jsx
import { useState, useEffect } from "react";

function App() {
  const [data, setData] = useState(null);

  useEffect(() => {
    async function fetchData() {
      const res = await fetch();
      const data = await res.json();
      setState(data);
    }

    fetchData();

    return () => console.log("Cleanup");
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

<br>

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

<br>

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## The useEffect Cleanup Function

```jsx
useEffect(() => {
  // code ...

  return () => "";
}, []);
```

`useEffect` cleanup function

- Function that we can return from an effect (optional)

- Runs on two different occasions:

  1. Before the effect is **executed again** (in order to cleanup the results of the previous side effect)
  2. After a component has **unmounted**, (in order to give us the opportunity to reset the side effect that we created if that's necessary)

<br>

- Component renders -> Execute effect if dependency array includes updated data
- Component unmounts -> Execute cleanup function

When we need a cleanup function? Whenever the side effect keeps happening after the component has been re-rendered un unmounted. Examples: start timer -> stop timer, add event listener -> remove listener, etc.

Each effect should do **only one thing.** Use one `useEffect` hook for each side effect. This makes effects easier to clean up.

If you need to create multiple effects, just use multiple useEffect hooks.

The cleanup function runs after the component has already unmounted, even so thanks to JS closures the cleanup function will remember the previous variables.

<br>

## Cleaning Up Data Fetching

_Race condition_

```jsx
useEffect(() => {
  const constroller = new AbortController();

  async function fetchData() {
    const res = fetch("apilink", { signal: controller.signal });
  }

  return () => controller.abort();
}, [dep]);
```

## Listening to a Keypress

Event listeners should be placed inside `useEffect` hook, because they are side effects.

Each time the same component mounts, a new event listener is added to the document, always an additional one to the ones that we already have, so we have to cleanup our event listeners.

```jsx
useEffect(() => {
  const callback = () => "";

  document.addEventListener("keydown", callback);

  return () => document.removeAddEventListenr("keydown", callback);
}, []);
```

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Clean Up Effects

Node: When we add event listeners to the DOM, it is important to remove those event listeners when we are done with them to avoid memory leaks!

Because effects run after every render and not just once, React calls our cleanup function before each re-render and before unmounting to clean up each effect call.

```js
useEffect(() => {
  btn.addEventListener("click", handleClick);

  return () => {
    btn.removeEventListener("click", handleClick);
  };
});
```

If our effect returns a function, then the `useEffect()` Hook always treats that as a cleanup function. React will call this cleanup function before the component re-renders or unmounts. Since this cleanup function is optional, it is our responsibility to return a cleanup function from our effect when our effect code could create memory leaks.
