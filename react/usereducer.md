# useReducer

The **`useReducer`** hook is an alternative to `useState`, used for managing complex state logic. Particularly useful when the state logic involves multiple sub-values or when the next state depends on the previous one.

```jsx
const initialState = { text: "Click" };

function reducer(state, action) {
  if (action.type === "update") return { text: action.payload };
}

export default function App() {
  const [state, dispatch] = useReducer(reducer, initialState);

  const update = () => dispatch({ type: "update", payload: "Cicked" });

  return <button onClick={update}>{state.text}</button>;
}
```

- **`useReducer`:** takes as arguments the `reducer` function and the `initialState` of the component, and returns the `state` and a `dispatch` function.

- **`reducer`:** The reducer function determines how the state changes in response to an `action`, it takes the current state and an action as arguments, and returns the new state.

- **`dispatch`:** Function to trigger state updates, by "sending" actions from event handlers to the `reducer`, it is provided by the `useReducer` hook.

- **`action`:** an object that describes how the state should be updated. It typically have a `type` property that describes the kind of change, and a `payload` property (optional) with data needed for the update.

<br>

## useState vs useReducer

### useState

- Ideal for **single, independent pieces of state** (numbers, strings, single arrays, etc.)
- Logic to update state is placed directly in event handlers or effects, **spread all over one or multiple components**
- State is updated by **calling `setState(true)`**
- **Imperative** state updates
- Easy to understand and to use

### useReducer

- Ideal for multiple **related pieces of state and complex state** (e.g. object with many values and nested objects or arrays)
- Logic to update state lives in **one central place, decoupled from components:** the reducer
- State is updated by **dispatching an action** to a reducer
- **Declarative** state updates: complex state transitions are mapped to actions `dispatch({ type: "startGame" })`
- More difficult to understand and implement

### When to use useReducer

The `useReducer` hook is the way to go if:

- You need multiple pieces of related state, including objects
- If states frequently update together
- Too many event handlers make components large and confusing

<br>

## Code Example

```jsx
import { useReducer } from "react";

const initialState = { count: 0, step: 1 };

function reducer(state, action) {
  switch (action.type) {
    case "dec":
      return { ...state, count: state.count - state.step };
    case "inc":
      return { ...state, count: state.count + state.step };

    case "setCount":
      return { ...state, count: action.payload };
    case "setStep":
      return { ...state, step: action.payload };

    case "reset":
      return { count: 0, step: 1 };

    default:
      throw new Error("Unknown Action");
  }
}

function App() {
  const [state, dispatch] = useReducer(reducer, initialState);
  const { count, step } = state;

  function dec() {
    dispatch({ type: "dec" });
  }

  function inc() {
    dispatch({ type: "inc" });
  }

  function defineCount({ target }) {
    dispatch({ type: "setCount", payload: +target.value });
  }

  function defineStep({ target }) {
    dispatch({ type: "setStep", payload: +target.value });
  }

  function reset() {
    dispatch({ type: "reset" });
  }

  return <p>{`${count} ${step}`}</p>;
}
```
