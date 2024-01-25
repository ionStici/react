## State Update Batching

Renders are not triggered immediately, but scheduled for when the JS engine has some "free time". There is also batching for multiple `setState` calls in event handlers.

```jsx
// Event handler function
const reset = () => {
  setCount(0);
  console.log(count); // 7
  setModal(false);
  setPlay(true);
};
```

Considering an event handle function with multiple state updates, these state updated will get batched into just **one** state update for the entire event handler.

Updating multiple pieces of state won't immediately cause a re-render for each update, instead all pieces of state inside the envet handler are updated in one go, and only then will React trigger one single _render + commit_.

What will the value of `count` be when logging it? At the point of logging the `count` state, this state will still hold the state before update. At this point we say that the state is stale, meaning that the state is no longer fresh and updated, because a state update will only be reflected in the state variable after the re-render. For this reason, we say that updating state in react is asynchronous.

Updated state variables are not immediately available after `setState` call, but only after the re-render.

This also applies when only one state variable is updated.

If we need to update state based on previous update, we use `setState` with a callback.

Starting with React 18, state batching is available in timeout functions, promises and DOM native event methods:

```jsx
setTimeout(reset, 1000);
fetchStuff().then(reset);
btn.addEventListener("click", reset);
```

p.s. If we attempt to update the state while the new state is equal to the current state, React will not update the state and not re-render the component instance.

p.s. In case we try to update the same state multiple times in a row by computing the new state from the current state, the next state updates actually will not use the updated state but instead the current state for each update. Because the state remains stale.

<br>
