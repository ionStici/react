# Context API

**Context API:** allows components everywhere in the tree to read state that a context shares; system to pass data throughout the app; it allows us to transmit global state to the entire app.

_Context API parts:_

1. **Provider:** gives all child components access to `value`
2. **Value:** the data that we want to make available (usually state and functions).
3. **Consumers:** all components that read the provided context `value`

Whenever the context `value` is updated, _all consumers re-render_.

```jsx
import { createContext, useContext, useState } from 'react';

// 1. Create Context
const ListContext = createContext(null);

export default function App() {
  const [list, setList] = useState([]);

  return (
    // 2. Provide the Context to child components
    <ListContext.Provider value={{ list, setList }}>
      <Component />
    </ListContext.Provider>
  );
}

function Component() {
  // 3. Consuming the Context
  const { list, setList } = useContext(ListContext);
}
```

## Context + useReducer with Custom Provider and Hook

_Code Example_

```jsx
import { useReducer, createContext, useContext } from 'react';

const initialState = { text: 'Click' };

const reducer = function (state, action) {
  switch (action.type) {
    case 'update':
      return { ...state, text: action.payload };
    default:
      throw new Error('Unknown Action');
  }
};

const DataContext = createContext();

function DataProvider({ children }) {
  const [state, dispatch] = useReducer(reducer, initialState);
  const handleClick = () => dispatch({ type: 'update', payload: 'Clicked' });

  return (
    <DataContext.Provider value={{ state, handleClick }}>
      {children}
    </DataContext.Provider>
  );
}

function useData() {
  const context = useContext(DataContext);
  if (!context)
    throw new Error('DataContext was used outside of the DataProvider');
  return context;
}

export default function App() {
  return (
    <DataProvider>
      <Component />
    </DataProvider>
  );
}

function Component() {
  const { state, handleClick } = useData();
  return <button onClick={handleClick}>{state.text}</button>;
}
```
