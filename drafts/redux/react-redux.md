[&larr; Back](./README.md)

# React Redux Library

```
npm install react-redux
```

## Table of Content

- [The Provider Component](#the-provider-component)
- [Selectors](#selectors)
- [useSelector() Hook](#useselector-hook)
- [useDispatch() Hook](#usedispatch-hook)

<br>

## The Provider Component

```js
// index.hs
import { App } from "./app/App.js";
import { store } from "./app/store.js";
import { Provider } from "react-redux";

root.render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

- Wrap the `<Provider>` component around the top-level component and pass `store` through the `store` prop of the Provider.
- Note that we no longer use the `render` function that we usually subscribe.

The `<Provider>` component gives the components of an app access to the Redux store without the need to pass the store directly to the React components through props.

<br>

## Selectors

Using the `<Provider>` component, the Redux `store` is provided to the React Components.

We need to define which data from the `store`, each component needs access to. We do this by creating _selector functions_ (user defined).

A _selector_ is a pure function (no side effects) that selects and returns data from the Redux store's state.

- Takes `state` as an argument.
- Returns what is needed by the component from `state`.

<br>

```js
export const selectFilter = (state) => state.filter;
export const selectTodoText = (state) => state.todos.map((todo) => todo.text);
```

_Convention:_ selector names start with `select` and then the piece of data we retrieve.

_Use case:_ Selectors are used together with the `useSelector` hook to retrieve the needed data.

<br>

## useSelector() Hook

- `useSelector` returns data from the `store` using selectors.
- It subscribes a child component of `<Provider />` to changes in the store.
- React will re-render the component if the data from the selector changes.

```js
// Todos.js
import { useSelector } from "react-redux";
import { selectTodos } from "todosSlice.js";

export const Todos = () => {
  const todos = useSelector(selectTodos);
  const todosById = useSelector((state) => state.todos[props.id]);

  return <p>{todos}</p>;
};
```

- We pass a selector function to `useSelector` and then it will return the selected data from the Redux store.
- Calling `useSelector` inside the component also subscribes the respective component to re-render if any changes occur in the `todos` portion of the Redux store. This optimizes the performance of the application by only re-rendering components that have had their data change and not the entire application.
- `useSelector` accepts inline selectors, this can be useful when we need to use props for data retrieval.

`useSelector()` completes the 3-step process for accessing data from the Redux store using `react-redux`:

1. The `<Provider>` component is used to provide the Redux store to the nested application.
2. Selectors are created to give instructions on retrieving data from the store.
3. `useSelector()` is called within a child component of `<Provider>` for executing selector instructions to retrieve data and subscribe to re-rendering.

<br>

## useDispatch() Hook

```js
import { useSelector, useDispatch } from "react-redux";
import { selectTodo } from "./todoSlice.js";
import { removeTodo } from "./todoSlice.js";

const Todo = () => {
  const todo = useSelector(selectTodo);
  const dispatch = useDispatch();

  return <button onClick={() => dispatch(removeTodo(todo))}>{todo}</button>;
};
```

The `useDispatch` hook allows you to dispatch actions from any component that is a descendent of the `<Provider>` component, avoiding passing a reference of `store.dispatch` through props. Both approaches accomplish the same thing but `useDispatch()` avoids props drilling.

<br>
