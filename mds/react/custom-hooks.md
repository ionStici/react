# Custom Hooks

## React Hooks and Their Rules

**The Rules of Hooks:** Hooks must be called at the top level, and only from functional components or custom hooks.

**React Hooks** are special built-in functions that allow us to "hook" into the React internal mechanism.

In other words, hooks are APIs that expose some internal React functionality, such as: creating and accessing state from the fiber tree; registering side effects in the fiber tree; manual DOM selections; etc.

Hooks always start with the word `use`. React comes with almost 20 built-in hooks:

_The most important hooks:_ `useState`, `useEffect`, `useReducer`, `useContext`.

_Less used hooks:_ `useRef`, `useCallback`, `useMemo`, `useTransition`, `useDeferredValue`.

_Other hooks:_ `useLayoutEffect`, `useDebugValue`, `useImperativeHandle`, `useId`.

## Custom Hooks

Reusing logic with custom hooks. Does logic contains any hooks? No, then use regular functions. Yes, then create a custom hook, which allows us to **reuse non-visual logic in multiple components**.

One custom hook should have only one purpose, to make it reusable and portable (even across multiple projects). Rules of hooks apply to custom hooks too.

- A custom hook is a JavaScript function, it can receive and return any data.
- Custom hooks need to use one or more React hooks.
- The function name of a custom hook needs to start with the word `use` (not optional).

### useLocalStorageState Custom Hook

```jsx
import { useEffect, useState } from "react";

export function useLocalStorageState(key, initialValue) {
  const [value, setValue] = useState(() => {
    const storedValue = localStorage.getItem(key);
    return storedValue ? JSON.parse(storedValue) : initialValue;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [value, key, initialValue]);

  return [value, setValue];
}

function App() {
  const [age, setAge] = useLocalStorageState("age", 25);
}
```
