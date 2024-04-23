# useRef

We should not select DOM elements using `querySelector` (even if it works), because React is declarative, and this is not the "React way" of doing things. Instead, we should use the `useRef` hook (ref stands for reference).

`useRef` creates an object with a mutable `.current` property that persists across renders (variable defined in the render logic will always reset). In this object we can put any data that we want to be preserved between renders.

```jsx
const myRef = useRef(23);
myRef.current = 1000;
```

_Two big use cases for `useRef`:_

1. Creating a variable that persists between renders (e.g. previous state, setTimeout id, etc.)
2. Selecting and storing DOM elements.

**Note:** Refs are for data that is NOT rendered, usually only appears in event handlers or effects, not in JSX, otherwise use state.

Do NOT write or read the `.current` property in the render logic, this would create an undesirable side effect, instead use it in a `useEffect` hook.

Main refs characteristics: it persists across renders, it doesn't cause re-renders, it is mutable, synchronous updates.

### Selecting DOM elements with useRef

```jsx
function App() {
  const inputEl = useRef(null);

  useEffect(() => {
    inputEl.current.focus();
  }, []);

  return <input ref={inputEl} />;
}
```

When we work with DOM elements, the initial value of `useRef` should be `null`.
