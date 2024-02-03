# Performance Optimization

## Table of Content

- []()

<br>

## Performance Optimization and Wasted Renders

1. **Prevent Wasted Renders:** `memo`; `useMemo`; `useCallback`; Passing elements as `children` or regular prop;

2. **Improve App Speed / Responsiveness:** `useMemo`; `useCallback`; `useTransition`;

3. **Reduce Bundle Size:** Using fewer 3rd-party packages; Code splitting and lazy loading;

A component instance only gets re-rendered in 3 different situations: (1) State Changes; (2) Context Changes; (3) Parent Re-renders;

**Note:** The real reason why a component re-renders when props change is that the parent has re-rendered.

**Remember:** a render does not mean that the DOM actually gets updated, it just means the component function gets called. But this can be an expensive operation.

**Wasted render:** a render that didn't produce any change in the DOM. This becomes a problem ONLY when renders they happen too frequently or when the component is very slow, resulting in not updating the UI fast enough after the user performs a certain action.

**Profiler Developer Tool:** Analyze renders and re-renders

<br>

## A Surprising Optimization Trick With children

```jsx
// laggy example

function SlowComponent() {
  // creates a list with 100_000 items
}

function Test() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount((c) => c + 1)}>Increase: {count}</button>
      <SlowComponent />
    </div>
  );
}
```

```jsx
// optimized example

function SlowComponent() {
  // creates a list with 100_000 items
}

function Counter({ children }) {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount((c) => c + 1)}>Increase: {count}</button>
      {children}
    </div>
  );
}

function Test() {
  return (
    <>
      <Counter>
        <SlowComponent />
      </Counter>
    </>
  );
}
```

In both examples the component tree is exactly the same.

**Laggy Example:** `SlowComponent` re-renders every time `Test` re-renders, _even though `SlowComponent` doesn't have any dependency on the `count` state_. This results in wasted renders.

**Optimized Example:** When `count` state changes, only `Counter` re-renders. `SlowComponent` being passed as `children`, _does not re-render because it is not a direct child of `Counter`_. This structure prevents `SlowComponent` from re-rendering unnecessarily.

<br>

## `memo`

**Memoization:** Optimization technique that executes a pure function once, and saves the result in memory (cache). If we try to execute the function again with the **same arguments as before**, the previously saved result will be returned (cached result), **without executing the function again**. We can use this technique in React to optimize our applications.

**Memoize** components with `memo`, objects with `useMemo`, functions with `useCallback`. This will help us prevent wasted renders and improve the overall application speed / responsiveness.

**`memo`** function is used to create a memoized component that will **not re-render when its parent re-renders**, as lon as the **props stay the same between renders.**

`memo` only affects props (input). A memoized component will still re-render when its own state changes or when a context that it's subscribed to changes.

Only makes sense when the component is **heavy** (slow rendering), **re-renders often**, and does so **with the same props**.

- Regular behavior (no memo): Component re-renders -> Child re-renders.

- Memoized child with `memo`: Component re-renders -> if same props: Memoized child does NOT re-render / if new props: memoized child re-renders.

### `memo` in Practice

```jsx
import { memo } from "react";

// memoized component
const SlowComponent = memo(function (props) {
  return null;
});
```

```jsx
export default memo(SlowComponent);
```

<br>

## Understanding useMemo and useCallback

### The Challenge of Re-rendering

- **Re-creating on Every Render:** In React, objects and functions are re-created on every re-render.

- **JavaScript's Comparison Logic:** In JavaScript, two separate instances of an object or function, even if identical in structure, are considered different (e.g. `{}` is not equal to `{}`)

- **Implications of Props:** When objects or functions are passed as props, the child component will always see them as new props on each re-render, potentially leading to unnecessary re-renders even if using `memo`

### The Solution: Memoization

**Memoization:** technique to preserve object and function instances across re-renders, ensuring they remain consistent (`memoized {}` is equal to `memoized {}`)

### React's Hooks for Memoization

- **Purpose:** `useMemo` is used to memoize values and `useCallback` to memoize functions between renders. The values are stored in memory ("cached") and are returned in later re-renders, as long as dependencies ("inputs") stay the same.

- **Dependency Array:** (similar to useEffect) The memoized value is only recalculated if its dpendency has changed since the last render.

### Three big use cases

1. Memoizing props to prevent wasted renders (together with `memo`)
2. Memoizing values to avoid expensive re-calculations on every render
3. Memoizing values that are used in dependency array of another hook

### `useMemo`

```jsx
import { memo, useCallback, useMemo, useState } from "react";

const [count, setCount] = useState(0);

const options = useMemo(() => {
  return { text: `Count; ${count}` };
}, [count]);
```

Whanever we return from this function will be saved in the cache.

If we leave the dependency array empty, the value will only be computed once in the beginning and will then never change.

### `useCallback`

```jsx
const handleAddPost = useCallback(function handleAddPost(post) {
  setPosts((posts) => [post, ...posts]);
}, []);
```

<br>

## Optimizing Context

We need to optimize a context in case three things are true at the same time: the state in the context needs to change all the time; the context has many consumers; the app is slow;

```jsx
function DataProvider({ children }) {
  const handleUpdate = () => "";

  const value = useMemo(() => {
    return { data, onUpdate: handleUpdate };
    0, [data];
  });

  return <DataContext value={value}>{children}</DataContext>;
}
```

<br>

## Don't Optimize Prematurely!

### DOs

- **Find performance bottlenecks using the Profiler and visual inspection (laggy UI)**
- Fix those real performance issues
- Memoize expensive re-renders
- Memoize expensive calculations
- Optimize context if it has many consumers and changes often
- Memoize context value + child components
- Implement code splitting + lazy loading for SPA routes

### DON'Ts

- **Don't optimize prematurely!**
- Don't optimize anything if there is nothing to optimize
- Don't wrap all components in memo
- Don't wrap all values in useMemo
- Don't wrap all functions in useCallback
- Don't optimize context if it's not slow and doesn't have many consumers

<br>
