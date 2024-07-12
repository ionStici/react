# Context API

**Context API:** allows components everywhere in the tree to read state that a context shares; system to pass data throughout the app; it allows us to transmit global state to the entire app.

_Context API parts:_

1. **Provider:** gives all child components access to `value`
2. **Value:** the data that we want to make available (usually state and functions).
3. **Consumers:** all components that read the provided context `value`

Whenever the context `value` is updated, _all consumers re-render_.

```jsx
// DataContext.jsx
import { createContext, useContext, useState } from "react";

// 1. Create Context
const DataContext = createContext(null);

// 2. Create Custom Context Provider
export default function DataProvider({ children }) {
  const [data, setData] = useState(null);

  return (
    <DataContext.Provider value={{ data, setData }}>
      {children}
    </DataContext.Provider>
  );
}

// 3. Create Custom useData Hook
export function useData() {
  return useContext(DataContext);
}
```

```jsx
import DataProvider, { useData } from "DataContext";

function App() {
  // 4. Provide the Context
  return (
    <DataProvider>
      <Component />
    </DataProvider>
  );
}

function Component() {
  // 5. Use the Context
  const { data, setData } = useData();
}
```
