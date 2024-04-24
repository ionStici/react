## State Update Batching

Renders are not triggered immediately, but scheduled for when the JS engine has some "free time". There is also batching for multiple `setState` calls in event handlers.

```jsx
const reset = () => {
  setCount(0);
  console.log(count); // 7 | and not 0
  setModal(false);
  setPlay(true);
};
```

Considering an event handle function with multiple state updates: these state updates will get batched into just ONE state update for the entire event handler.

Updating multiple pieces of state won't immediately cause a re-render for each update, instead all pieces of state inside the event handler are updated in one go, and only then will React trigger one single _render + commit_.

Because of this, the value of `count` at the moment of logging it, **will be stale**, that's why we say that updating state in react is asynchronous. If we need to update the state based on the previous state value, we should use `setState` with a callback.

Updated state variables are not immediately available after the `setState` call, but only after the re-render. This also applies when only one state variable is updated.

**Note:** If we attempt to update the state using the same state value as the current state, React will not update it and will not re-render the component instance.

---

Starting with React 18, state batching is available in timeout functions, promises and DOM native event methods:

```jsx
setTimeout(reset, 1000);
fetchStuff().then(reset);
btn.addEventListener('click', reset);
```
