# Custom Hooks

## React Hooks and Their Rules

**The Rules of Hooks:** Hooks must be called at the top level, and only from functional components or custom hooks.

**React Hooks** are special built-in functions that allow us to "hook" into the React internal mechanism.

In other words, hooks are APIs that expose some internal React functionality, such as: creating and accessing state from the fiber tree; registering side effects in the fiber tree; manual DOM selections; etc.

Hooks always start with the work `use`. React comes with almost 20 built-in hooks:

_The most important hooks:_ `useState`, `useEffect`, `useReducer`, `useContext`.

_Less used hooks:_ `useRef`, `useCallback`, `useMemo`, `useTransition`, `useDeferredValue`.

_Other hooks:_ `useLayoutEffect`, `useDebugValue`, `useImperativeHandle`, `useId`.

## Custom Hooks

Reusing logic with custom hooks. Does logic contains any hooks? No, then use regular functions. Yes, then create a custom hook, which allows us to **reuse non-visual logic in multiple components**.

One custom hook should have only one purpose, to make it reusable and portable (even across multiple projects). Rules of hooks apply to custom hooks too.

- A custom hook is a JavaScript function, it can receive and return any data.
- Custom hooks need to use one or more React hooks.
- The function name of a custom hook needs to start with the word `use` (not optional).

### useLocalStorageState

```jsx
import { useEffect, useState } from 'react';

function useLocalStorageState(key, initialValue) {
  const [value, setValue] = useState(() => {
    const storedValue = localStorage.getItem(key);
    return storedValue ? JSON.parse(storedValue) : initialValue;
  });

  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [value, key, initialValue]);

  return [value, setValue];
}

export { useLocalStorageState };

function App() {
  const [test, setTest] = useLocalStorageState('test', [0]);
}
```

### useKeypress

```jsx
function useKeypress(key, action) {
  useEffect(() => {
    const callback = e => {
      if (e.code.toLowerCase() === key.toLowerCase()) action();
    };

    document.addEventListener('keydown', callback);
    return () => document.removeEventListener('keydown', callback);
  }, [action, key]);
}

function App() {
  const log = () => console.log('success');
  useKeypress('Enter', log);
  return null;
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
      return setError('Your browser does not support geolocation');
    }

    setIsLoading(true);

    navigator.geolocation.getCurrentPosition(
      location => {
        setPosition({
          lat: location.coords.latitude,
          lng: location.coords.longitude,
        });
        setIsLoading(false);
      },
      error => {
        setError(error.message);
        setIsLoading(false);
      }
    );
  }

  return { isLoading, position, error, getPosition };
}

function App() {
  const { isLoading, position, error, getPosition } = useGeolocation();
  const handleClick = () => getPosition();

  useEffect(() => {
    console.log(isLoading, position, error);
  }, [isLoading, position, error]);

  return <button onClick={handleClick}>Click</button>;
}
```
