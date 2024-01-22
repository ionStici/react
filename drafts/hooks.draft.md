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
