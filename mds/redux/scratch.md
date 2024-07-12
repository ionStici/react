# createStore from Scratch

```js
function createStore(reducer) {
  let state;
  let listeners = [];

  const getState = () => state;

  const dispatch = (action) => {
    state = reducer(state, action);
    listeners.forEach((listener) => listener());
  };

  const subscribe = (listener) => {
    listeners.push(listener);
    return () => (listeners = listeners.filter((l) => l !== listener));
  };

  dispatch({ type: "INIT" });
  return { getState, dispatch, subscribe };
}
```

```js
const initialState = { name: "" };

function reducer(state = initialState, action) {
  switch (action.type) {
    case "updateName":
      return { ...state, name: action.payload };

    default:
      return state;
  }
}

const UPDATE_NAME = "updateName";

const updateName = (name) => {
  return { type: UPDATE_NAME, payload: name };
};

const store = createStore(reducer);

const dispatch = store.dispatch;
const subscribe = store.subscribe;
const selectName = () => store.getState().name;

const log = () => console.log(selectName());
const unsubscribe = subscribe(log);

dispatch(updateName("John"));
unsubscribe();

export { updateName, dispatch, subscribe, selectName };
```
