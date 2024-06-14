# useReducer

The **`useReducer`** hook is an alternative to `useState` used for managing complex state logic. Particularly useful when the state logic involves multiple sub-values or when the next state depends on the previous one.

```jsx
import { useReducer } from 'react';

const initialState = { text: 'Click' };

const reducer = (state, action) => {
  switch (action.type) {
    case 'update':
      return { ...state, text: 'Clicked' };
    default:
      throw new Error('UnKnown Error');
  }
};

function App() {
  const [state, dispatch] = useReducer(reducer, initialState);
  const handleClick = () => dispatch({ type: 'update', payload: 'Clicked' });

  return <button onClick={handleClick}>{state.text}</button>;
}
```

- **`useReducer`:** takes as arguments the `reducer` function and the `initialState` of the component, and returns the `state` and a `dispatch` function.

- **`reducer`:** The reducer function determines how the state changes in response to an `action`, it takes the current state and an action as arguments, and returns the new state.

- **`dispatch`:** Function to trigger state updates, by "sending" actions from event handlers to the `reducer`, it is provided by the `useReducer` hook.

- **`action`:** an object that describes how the state should be updated. It typically have a `type` property that describes the kind of change, and a `payload` property (optional) with data needed for the update.
