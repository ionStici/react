[&larr; Back](./README.md)

# Strategies for Complex State

## Table of Content

- [Slices](#slices)
- [Slices and Actions](#slices-and-actions)
- [Reducer: Immutable Updates](#reducer-immutable-updates)
- [Reducer Composition](#reducer-composition)
- [combineReducers](#combinereducers)
- [Redux Structure](#redux-structure)
- [Passing Store Data](#passing-store-data)
- [Feature Components](#feature-components)

<br>

## Slices

- **Slices:** top-level state properties.
- Each _slice_ represents a different feature of the entire app.

```js
const initialState = {
  todos: [],
  filter: true,
  searchTerm: "",
};
```

The `initialState` general purposes:

1. Plan out the general structure of the state.
2. Provide an initial state value to the reducer function.

<br>

## Slices and Actions

- State with multiple slices - individual actions only change one slice at a time.
- The `type` property follows the pattern: `'sliceName/actionDescriptor'`.

```js
store.dispatch({ type: "searchTerm/setSearchTerm", payload: "homework" });
store.dispatch({ type: "searchTerm/clearSearchTerm" });
```

`payload` - additional data passed to the reducer in order to carry out the desired change-of-state.

<br>

## Reducer: Immutable Updates

- The reducer is called each time an action is dispatched to the store.
- The reducer receives the action and the current state as arguments and returns the next state.

```js
return {
  ...state,
  todos: [...state.todos, action.payload],
};
```

**Rule of Reducers:** when the reducer is updating the state, it must make a copy and return the copy rather than directly mutating the incoming state. When the state is a mutable data type, like an array of object, this is done using the spread operator.

- Immutable state means that you never modify state directly, instead, you return a new copy of state.
- When a change is made to the state, we spread the old state's content into a new object before producing the change.
- The same is true for the inner array, first we spread the entire array items into a new array, and then add a new item.
- The `map()` method is a great tool for achieving the same results as the spread operator, since it returns a new array.

<br>

## Reducer Composition

- **Reducer Composition Pattern** - separate reducers responsible for individual slices.
- All the _slice reducers_ are recombined by a `rootReducer` to form a single state object.

```js
const initialTodos = [];
const todosReducer = (todos = initialTodos, action) => {};

const initialFilter = "";
const filterReducer = (filter = initialFilter, action) => {};

const rootReducer = (state = {}, action) => {
  const nextState = {
    todos: todosReducer(state.todos, action),
    filter: filterReducer(state.filter, action),
  };
  return nextState;
};

const store = createStore(rootReducer);
```

When an `action` is dispatched to the `store`:

- The `rootReducer` will call each _slice reducer_ with that `action` and the appropriate slice of the state as arguments.
- The slice reducers each determine if they need to update their slice of state, or simply return their slice of state unchanged.
- The `rootReducer` reassembles the updated slice values in a new state object.

The `initialState` object has been replaced by individual `initialSliceName` variables which are used as default values for the state of each slice reducer. Then, all slice reducers are called within `rootReducer` to update their slices of state.

<br>

## combineReducers

```js
import { createStore, combineReducers } from "redux";

const reducers = {
  todos: todosReducer,
  filter: filterReducer,
};

const rootReducer = combineReducers(reducers);
const store = createStore(rootReducer);
```

- `combineReducers()` accepts an object of reducers and returns a `rootReducer` function.
- The returned `rootReducer` is passed to `createStore()` to create the `store` object.

When an action is dispatched to the `store`, the `rootReducer` is executed which then calls each slice reducer and pass along the action and the appropriate slice of state.

<br>

## Redux Structure

Break up the Redux app using the [Redux Ducks Pattern](https://github.com/erikras/ducks-modular-redux).

```
src/
|-- index.js
|-- app/
    |-- App.js
    |-- store.js
|-- features/
    |-- featureA/
        |-- featureASlice.js
        |-- featureA.js
    |-- featureB/
        |-- featureBSlice.js
        |-- featureB.js
```

- `index.js` is the entry point to the entire app.
- `app/store.js` purpose is to create the `rootReducer` and the Redux `store`.
- The `features/` directory, and its subdirectories, contain the code relating to each individual slice.

<br>

## Passing Store Data

Passing Store Data Through the Top-Level `<App />` React Component

The `<App />` top-level React component, will render each feature-component and pass the data needed by those components as prop values.

In Redux applications, we pass data to components: state slices and dispatch methods for triggering state changes through user interactions within the component.

The distribution of `store.dispatch` methods and the slices of state to all of the feature-components, via the `<App />` component, begins in the `index.js` file.

<br>

```js
// index.js
import { App } from "./app/App.js";
import { store } from "./app/store.js";

const root = createRoot(document.getElementById("root"));

const render = () => {
  root.render(<App state={store.getState()} dispatch={store.dispatch} />);
};
render();

store.subscribe(render);
```

```js
// app.js
import { Comp1 } from "./comp1.js";
import { Comp2 } from "./comp2.js";

export function App(props) {
  const { state, dispatch } = props;

  return (
    <>
      <Comp1 slice1={state.slice1} dispatch={dispatch} />
      <Comp2 slice2={state.slice2} dispatch={dispatch} />
    </>
  );
}
```

<br>

## Feature Components

Using Store Data Within Feature Components.

Plugging in a feature-component to a Redux application involves the following steps:

- Import the React feature-components into the top-level App.js file.
- Render each feature-component and pass along the slice of state and the dispatch method as props.
- Within each feature-component:
  - Extract the slice of state and dispatch from props.
  - Render the component using data from the slice of state.
  - Import any action creators from the associated slice file.
  - Dispatch actions in response to user inputs within the component.

```js
// Comp1.js
// 1. Render Component with state slice data
// 2. Dispatch Action based on user events
```

```js
// Comp1_Slice.js
// 1. Action Creators
// 2. initialSliceState
// 3. Slice Reducer
```

<br>
