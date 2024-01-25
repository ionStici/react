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

### useLocalStorageState

```jsx
import { useState, useEffect } from "react";

function useLocalStorageState(initialState, key) {
  const [value, setValue] = useState(() => {
    const storedValue = localStorage.getItem(key);
    return storedValue ? JSON.parse(storedValue) : initialState;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [value, initialState, key]);

  return [value, setValue];
}

export { useLocalStorageState };

function App() {
  const [data, setData] = useLocalStorage([], "data");
}
```

### useKey

```jsx
import { useEffect } from "react";

function useKey(key, action) {
  useEffect(() => {
    const callback = (e) => {
      if (e.code.toLowerCase() === key.toLowerCase()) action();
    };

    document.addEventListener("keydown", callback);
    return () => document.removeEventListener("keydown", callback);
  }, [key, action]);
}

export { useKey };

function App() {
  useKey("Enter", () => {});
}
```

### useGeolocation

```jsx
function useGeolocation() {
  const [isLoading, setIsLoading] = useState(false);
  const [position, setPosition] = useState({});
  const [error, setError] = useState(null);

  function getPosition() {
    if (!navigator.geolocation) {
      return setError("Your browser does not support geolocation");
    }

    setIsLoading(true);
    navigator.geolocation.getCurrentPosition(
      (loc) => {
        setPosition({
          lat: loc.coords.latitude,
          lng: loc.coords.longitude,
        });
        setIsLoading(false);
      },
      (error) => {
        setError(error.message);
        setIsLoading(false);
      }
    );
  }

  return { isLoading, position, error, getPosition };
}
```

<br>
<br>
<br>

## Rules of Hooks

Two main rules when using Hooks:

- Only call Hooks at the top level
- Only call Hooks from React functions

[Hooks API Reference](https://reactjs.org/docs/hooks-reference.html)

When React builds the Virtual DOM, the library calls the functions that define our components over and over again as the user interacts with the user interface. React keeps track of the data and functions that we are managing with Hooks based on their order in the function component’s definition. For this reason, we always call our Hooks at the top level; we never call hooks inside of loops, conditions, or nested functions.

Secondly, Hooks can only be used in React Functions. We cannot use Hooks in class components and we cannot use Hooks in regular JavaScript functions. We’ve been working with useState() and useEffect() in function components, and this is the most common use. The only other place where Hooks can be used is within custom hooks. Custom Hooks are incredibly useful for organizing and reusing stateful logic between function components. For more on this topic, head to the [React Docs](https://reactjs.org/docs/hooks-custom.html).
