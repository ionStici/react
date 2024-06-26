# Redux Toolkit

```js
// npm install @reduxjs/toolkit
import { createSlice, configureStore } from '@reduxjs/toolkit';
```

## createSlice

```js
const options = {
  name: 'todos',
  initialState: [],
  reducers: { addTodo: (state, action) => [...state, action.payload] },
};

const todosSlice = createSlice(options);
```

- **`createSlice`** receives `options` object as an argument

  - `name` is a string that is used as the prefix for the generated action types, example: `'todos/addTodo'`
  - `initialState` is the initial state value for the current slice reducer.
  - `reducers` (referred to as _case reducers_) is an object of reducers. The keys determine the action types.

<div></div>

- **Case Reducers**

  - The property name of the reducers should correspond to an action type that the slice can handle.
  - Each case reducer should have 2 parameters, `state` and `action`, and return the next state.

<div></div>

- **Advantages** of redux-toolkit

  - Write reducers as functions inside of an object, instead of having to write a switch statement.
  - Action creators that correspond to each case reducer we provide will be automatically generated.
  - No default handler needs to be written.

<div></div>

- **Immer**

  - Redux Toolkit's `createSlice` function uses [Immer](https://immerjs.github.io/immer/) library which allow us to safely "mutate" the state.

  ```js
  addTodo: (state, action) => state.push({ ...action.payload });
  ```

  - Immer uses a special JS object called a Proxy to wrap the data you provide and lets you write code that “mutates” that wrapped data. Immer does this by tracking all the changes you’ve made and then uses that list of changes to return an immutably updated value as if you’d written all the immutable update logic by hand.

## Return Object - Actions

`createSlice` returns an object that looks like this:

```js
{
  name: "todos",
  reducer: (state, action) => newState,
  actions: {
    addTodo: (payload) => ({ type: 'todos/addTodo', payload }),
  }
  // case reducers field omitted
}
```

- `reducer` is the complete reducer function.
- `actions` holds the the auto-generated action creators.

### Auto-generated Action Creators

```js
todosSlice.actions.addTodo('do that');
// { type: 'todos/addTodo', payload: 'do that' }
```

- By default, action creators accept one argument, which it puts into the action object as `action.payload`.
- The `action.type` string is generated by combining the `name` field with the key of the case reducer function.

### Export Action Creators

```js
export const { addTodo, toggleTodo } = todosSlice.actions;
```

Redux community code convention, called the "ducks" pattern, suggests that: use named exports for the action creators and export them separately from the reducer.

## Return Object - Reducers

- `todosSlice.reducer` is the complete slice reducer function.

When an action with the type `'todos/addTodo'` is dispatched, `todosSlice` will executed `todosSlice.reducer()` to check if the dispatched action's `type` matches one of `todos.actions` case reducers. If so, it will run the matching case reducer function. If not, it will return the current state. This is exactly the same pattern that we had previously implemented with the `switch` statement.

```js
export default todosSlice.reducer;
```

We need to export `todosSlice.reducer` so that it can be passed to the store and be used as the `todos` slice of the state.

While `todosSlice.actions` are exported as named exports, `todosSlice.reducer` should be exported as a default export.

## configureStore

[`configureStore()`](https://redux-toolkit.js.org/api/configureStore) accepts a single configuration object parameter.

This object should have a reducer property that defines either a function to be used as the root reducer, or an object of slice reducers which will be combined to create a root reducer.

```js
import todosReducer from './todosSlice';
import filtersReducer from './filtersSlice';

const store = configureStore({
  reducer: {
    todos: todosReducer,
    filters: filtersReducer,
  },
});

export default store;
```

- It creates the root reducer from combining the provided case reducers, removing the need to call `combineReducers`.
- It creates the Redux store using that root reducer.
- It automatically adds the thunk middleware / checks for common mistakes like accidentally mutating the state.
- It automatically sets up the Redux DevTools Extension connection.
