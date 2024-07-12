# Middleware and Thunks

Async operations in Redux: [Thunk middleware for Redux](https://github.com/reduxjs/redux-thunk)

```bash
npm i redux-thunk
```

**Middleware in Redux:** Custom logic that can intercept, modify, or even cancel actions before they reach the reducer. Function that sits between dispatching an action and the store. Middleware is used for logging actions, performing asynchronous tasks, handling side effects, and more.

**Thunk Middleware:** A "thunk" is a specific type of Redux middleware used for handling asynchronous logic. It allows you to write action creators that return a function instead of an action object. This function can then be used to dispatch other actions, access the current state, or perform asynchronous requests.

**Setting up the Redux Store:**

```jsx
// store.js
import { configureStore, createSlice } from '@reduxjs/toolkit';

const userSlice = createSlice({
  name: 'user',
  initialState: { data: null, loading: false, error: null },
  reducers: {
    fetchUserStart(state) {
      state.loading = true;
      state.error = null;
    },
    fetchUserSuccess(state, action) {
      state.data = action.payload;
      state.loading = false;
    },
    fetchUserFailure(state, action) {
      state.error = action.payload;
      state.loading = false;
    },
  },
});

export const { fetchUserStart, fetchUserSuccess, fetchUserFailure } =
  userSlice.actions;

const store = configureStore({
  reducer: {
    user: userSlice.reducer,
  },
});

export default store;
```

**Creating a Thunk for Asynchronous Actions:**

```jsx
// thunks.jsx
import { fetchUserStart, fetchUserSuccess, fetchUserFailure } from './store';

function fetchUserData() {
  return async dispatch => {
    dispatch(fetchUserStart());
    try {
      const res = await fetch('https');
      const data = await res.json();
      dispatch(fetchUserSuccess(data));
    } catch (error) {
      dispatch(fetchUserFailure('Failed to fetch data'));
    }
  };
}

export default fetchUserData;
```

**React Component to Display Data:**

```jsx
// UserComponent.js
import React, { useEffect } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import fetchUserData from './thunks';

function UserComponent() {
  const dispatch = useDispatch();
  const { data, loading, error } = useSelector(state => state.user);

  useEffect(() => {
    dispatch(fetchUserData());
  }, [dispatch]);

  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error}</div>;
  return <h1>{data.name}</h1>;
}
```

- `UserComponent` dispatches the `fetchUserData` thunk when the component mounts.
- `fetchUserData` is a **thunk** that fetches user data from an API. It dispatches different actions based on whether the fetch is successful of fails.
- `fetchUserData` is a **thunk** because it returns an asynchronous function that dispatches actions based on the success or failure of an API request.

## Thunk

A `thunk` is a function that wraps an expression to delay its evaluation. In the context of Redux:

- **Function that Returns a Function:** A thunk in Redux is typically a function that returns another function. The inner function takes `dispatch` and `getState` as arguments. This allows the function to dispatch actions and access the current state.
- **Asynchronous Operations:** Thunks are used to handle asynchronous operations within Redux. They allow side effects (like API calls) to be handled by dispatching actions based on the result of these operations.

## Middleware

Middleware is a way to enhance Redux with additional functionality:

- **Role of Middleware:** Middleware acts as a layer between dispatching an action and the moment it reaches the reducer. It can modify actions, delay them, replace them, or even stop them before they reach reducers.
- **Redux Thunk Middleware:** Allows you to write action creators that return functions (thunks) instead of actions. This middleware handles these thunks by providing the `dispatch` and `getState` functions.

Redux Thunk middleware is implicitly included when you use `configureStore` from Redux Toolkit. It enables the dispatching of function-type actions, which are your thunks, to handle asynchronous logic or complex synchronous logic that needs access to the store's dispatch method or state.
