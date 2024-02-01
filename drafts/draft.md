# Draft

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
