# Redux Refresher

```bash
npm install @reduxjs/toolkit react-redux
```

## createSlice

```jsx
// counterSlice.js
import { createSlice } from '@reduxjs/toolkit';

const options = {
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    increment: state => {
      state.value += 1;
    },
    incrementByAmount: (state, action) => {
      state.value += action.payload;
    },
  },
};

const counterSlice = createSlice(options);
const counterReducer = counterSlice.reducer;

export const { increment, incrementByAmount } = counterSlice.actions;
export default counterReducer;
```

## configureStore

```jsx
// store.js
import { configureStore } from '@reduxjs/toolkit';
import counterReducer from 'counterSlice';

const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
});

export default store;
```

## Provider

```jsx
// main.jsx
import store from './store';
import { Provider } from 'react-redux';

return (
  <Provider store={store}>
    <App />
  </Provider>
);
```

## Render Component

```jsx
// Component.jsx
import { useSelector, useDispatch } from 'react-redux';
import { increment, incrementByAmount } from './counterSlice';

function Component() {
  const selectCount = state => state.counter.value;
  const count = useSelector(selectCount);

  const dispatch = useDispatch();

  const inc = () => dispatch(increment());
  const incByAmount = amount => dispatch(incrementByAmount(amount));

  return null;
}
```
