# Draft

## React Hooks and Their Rules

React hooks are special built-in functions that allow us to "hook" into the React internal mechanisms.

Hooks are APIs that expose some internal React functionality, such as: creating and accessing state from fiber tree; registering side effects in the fiber tree; manual DOM selections; etc.

The fiber tree is somewhere deep inside React, and usually not accessible to us at all, but using hooks we can essentially hook into that internal mechanism.

Hooks always start with the word `use`

We can even create our own custom hooks.

Hooks give function components the ability to own state and run side effects at different lifecycle points.

React comes with almost 20 built-in hooks.

One of the most important and most used hooks: `useState`, `useEffect`, `useReducer`, `useContext`.

less used hooks: `useRef`, `useCallback`, `useMemo`, `useTransition`, `useDeferredValue`. Hooks we will not learn: `useLayoutEffect`, `useDebugValue`, `useImperativeHandle`, `useId`

**The Rules of Hooks**: Hooks must be called at the top level and only from React functions

<br>

## useState

The initial value of `useState` that we pass only matter on the initial render.

## Initializing State With a Callback (Lazy Initial State)

We can initialize the state with a callback function that returns a value.

This function as well is considered only on the initial render.

Whenever the initial value of the `useState` hook depends on some sort of computation, we should pass in a callback function that React will execute on its initial render.

Local Storage in React:

```jsx
const [data, setData] = useState(() => {
  const storedData = localStorage.getItem("data");
  return JSON.parse(storedData);
});

useEffect(() => {
  localStorage.setItem("data", JSON.stringify(data));
}, [data]);
```

<br>

## useRef

We should not select DOM elements like this (even if it works):

```jsx
useEffect(() => {
  const el = document.querySelector(".input");
  el.focus();
}, []);
```

React is declarative, and this is not the React way of doing things.

Instead, we should use the `useRef` hook

Ref stands for reference.

- "Box" (object) with a mutable `.current` property that is persisted across renders ("normal" variables are always reset). In this object we can put any data that we want to be preserved between renders.

```jsx
const myRef = useRef(23);
myRef.current = 1000;
```

When we use `useRef`, React will give us an object with a mutable `current` property, we can then write any data into this `current` property (and read from it).

So, the `current` property's value stays the same between multiple renders.

2 big use cases for `useRef`:

1. Creating a variable that stays the same between renders (e.g. previous state, setTimeout id, etc.)
2. Selecting and storing DOM elements.

Refs are for data that is NOT rendered: usually only appear in event handlers or effects, not in JSX (otherwise use state)

Do NOT write or react `.current` in render logic (like state), this would create an undesirable side effect, instead use a `useEffect` hook.

|       | Persists across renders | Updating causes re-render | Immutable | Asynchronous Updates |
| ----- | :---------------------: | :-----------------------: | :-------: | :------------------: |
| State |           ✅            |            ✅             |    ✅     |          ✅          |
| Refs  |           ✅            |            ❌             |    ❌     |          ❌          |

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

<br>

## Custom Hooks

Reusing logic with custom hooks. Does logic contain any hooks? No, use regular functions. Yes, use custom hooks, which allow us to reuse non-visual logic in multiple components.

One custom hook should have one purpose, to make it reusable and portable (even across multiple projects).

Rules of hooks apply to custom hooks too.

A custom hook is just a JS function, it can receive and return any data. Custom hooks need to use one or more React hooks. The function name needs to start with the word `use` (not optional).
