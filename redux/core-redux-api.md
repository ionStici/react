[&larr; Back](./README.md)

# Core Redux API

## Table of Content

- [Introduction / Terminology](./intro.md)
- [createStore](#createstore)
- [Action Objects and Action Creators](#action-objects-and-action-creators)
- [The Reducer Function](#the-reducer-function)
- [Dispatch Actions to the Store](#dispatch-actions-to-the-store)
- [getState](#getstate)
- [subscribe](#subscribe)
- [unsubscribe](#unsubscribe)

<div></div>

<br>

## createStore

```js
// npm install redux
import { createStore } from "redux";

const initialState = true;
const reducer = (state = initialState, action) => state;

const store = createStore(reducer);
```

Create the `store` object using the `createStore` helper function. It receives the reducer function as its only argument.

- The **`store`** object holds the entire state of an application.
- It provides useful methods for interacting with its state.

<br>

## Action Objects and Action Creators

- State updates are triggered by _dispatching actions_.
- **Actions** are object that contain information about a desired action event.

<div></div>

1. Action Objects must contain the `type` property equal to a string that reflects the type of a planned event.
2. And then optionally a `payload` property with some data which is sent to the `store`.

```js
const action = { type: "addTodo", payload: "do that" };
```

### Action Creators

**Action Creators** are functions that create and return action objects.

Action Creators are called and passed directly to the `store.dispatch()` method.

```js
const toggle = () => ({ type: "toggle" });
store.dispatch(toggle());
```

Before the reducer of an application is even written, we should write action creators as a way of planning out which actions will be available to dispatch to the store.

### Common Practice

Assign action types as read-only constants, then reference these constants wherever they are used.

```js
const TOGGLE_LIST = "toggle";
const toggle = () => ({ type: TOGGLE_LIST });
```

_Convention:_ write these constants in uppercase and underscores.

<br>

## The Reducer Function

The **Reducer** function is responsible for the state modifications that take place in response to actions.

- The reducer takes the `state` and an action as arguments.
- At first, the `state` must receive a default `initialState` that we hard code.
- The `store` object will take the `initialState` value and render the UI with that data.
- The reducer must return a new copy of state and never modify the state directly.

```js
const initialState = { filter: false, items: [] };
const reducer = (state = initialState, action) => {
  switch (action.type) {
    case "filterOn":
      return { ...state, filter: true };

    case "filterOff":
      return { ...state, filter: false };

    default:
      return state;
  }
};
```

- In the `switch` statement, the reducer will check for the `type` of the desired state change.
- After matching the `type` action, it will produce and return the state change as written.
- If nothing macthes, it will just return the current state.

<br>

## Dispatch Actions to the Store

`store.dispatch()` method is used to dispatch an action to the store and then update the state.

```js
store.dispatch(toggle());
```

- As argument we pass in an action object with a `type` property describing the desired state change.
- This will execute the reducer function with the same action object.
- Assuming that `action.type` is recognized by the reducer, the state will be updated and returned.

<br>

## getState

`store.getState()` returns the current value of the store's state.

<br>

## subscribe

`store.subscribe(listener)` method allow us to register a _listener_ function that will be executed in response to changes made to the state.

```js
const reactToChange = () => console.log(store.getState());
store.subscribe(reactToChange);
```

Each time an action is dispatched to the `store` and a change to the state occurs, the subscribed listener will be executed.

The `subscribe` method is used to render the UI using changes made to the state.

### React and Redux

- Subscribe a _listener_ function to the `store` that will re-render the top-level React Component.
- This component will receive and pass on the current state `store.getState()` through props.
- All the components will use this data to render the UI.
- Event listeners attached to React components will dispatch actions to the `store`.

```js
function App(props) {
  store.subscribe(renderListener);
}

const renderListener = () => {
  root.render(<App state={store.getState()} />);
};

renderListener();
```

<br>

## unsubscribe

```js
const unsubscribe = store.subscribe(reactToChange);
unsubscribe();
```

`store.subscribe()` returns an `unsubscribe` function that if called it will stop the listener from responding to changes made to the `store`.

<br>
