# Context

## Table of Content

- [Context API](#context)

- [Creating, Providing, Consuming a Context](#creating-providing-consuming-a-context)

- [Custom Provider and Hook](#custom-provider-and-hook)

- [Advanced State Management](#advanced-state-management)

<br>

## Context API

**Context API:** allows components everywhere in the tree to read state that a context shares; system to pass data throughout the app; it allows us to transmit global state to the entire app.

_Context API parts:_

1. **Provider:** gives all child components access to `value`
2. **Value:** the data that we want to make available (usually state and functions). The data that we want to transmit through the provider, so we pass it into the provider
3. **Consumers:** all components that read the provided context `value`

Whenever the context `value` is updated, _all consumers re-render_.

<br>

## Creating, Providing, Consuming a Context

```jsx
import { createContext, useContext } from "react";

// 1. Create a Context
const PostsContext = createContext(null);

function App() {
  const [posts, setPosts] = useState([]);
  const handleAddPost = (post) => setPosts((posts) => [...posts, post]);
  const handleClearPosts = () => setPosts([]);

  return (
    // 2. Provide Value to Child Components
    <PostsContext.Provider
      value={{
        posts,
        onAddPost: handleAddPost,
        onClearPosts: handleClearPosts,
      }}
    >
      <Components />
    </PostsContext.Provider>
  );
}

function Components() {
  // 3. Consuming Context Value
  const context = useContext(PostsContext);
  const { posts } = useContext(PostsContext);
}
```

<br>

## Custom Provider and Hook

```jsx
import { createContext, useContext, useState } from "react";

// 1. Create Context
const DataContext = createContext(null);

// 2. Create Custom Provider
function DataProvider({ children }) {
  const [data, setData] = useState([]);
  const handleAddData = (el) => setData((prev) => [...prev, el]);

  return (
    <DataContext.Provider value={{ data, onAddData: handleAddData }}>
      {children}
    </DataContext.Provider>
  );
}

// 3. Create Custom Hook
function useData() {
  const context = useContext(DataContext);
  if (context === undefined)
    throw new Error("Data Context was used outside of the PostProvider");
  return context;
}

export { DataContext, useData };

// 4. Use Context
function App() {
  const { data, onAddData } = useData();
}
```

<br>

## Advanced State Management

| Nr. | Where to place State? | Tools                                    | When to use?                         |
| :-: | --------------------- | ---------------------------------------- | ------------------------------------ |
| 1.  | Local Component       | `useState` `useReducer` `useRef`         | Local State                          |
| 2.  | Parent Component      | `useState` `useReducer` `useRef`         | Lifting Up State                     |
| 3.  | Context               | Context API + `useState` or `useReducer` | Global State (preferably UI state)   |
| 4.  | 3rd-party library     | Redux, React Query, SWR, Zustand, etc.   | Global State (remote or UI)          |
| 5.  | URL                   | React Router                             | Global State (passing between pages) |
| 6.  | Browser               | Local Storage, Session Storage, etc.     | Storing data in user's browser       |

### State Accessibility

- **Local State:** Needed only be one or few components; Only accessible in component and child components;

- **Global State:** Might be needed by many components; Accessible to every component in the application;

### State Domain

- **Remote State:** All application data loaded from a remote server (API); Usually asynchronous; Needs re-fetching and updating;

- **UI State:** Theme, list filters, form data, etc; Usually synchronous and stored in the application;

### Manage State of Different Types

- **Local State + UI State:** `useState` `useReducer` `useRef`

- **Local State + Remote State:** `fetch` + `useEffect` + `useState` or `useReducer` (_Mostly in small apps_)

- **Global State + UI State:** Context API + `useState` or `useReducer` & Redux & React Router

- **Global State + Remote State:** Context API + `useState` or `userReducer` & Redux & Tools highly specialized in handling remote state: React Query, SWR.
