# Redux Async Actions with Middleware and Thunks

**Middleware** - is the code that runs in the middle - usually between a framework receiving a request and producing a response.

In Redux, middleware runs between the moment when an action is dispatched and the moment when that action is passed along to the reducer.

When we add middleware to a Redux project, Redux passes dispatched actions to the middleware before passing them to the reducers.

Middleware intercepts actions after they are dispatched and before they are passed along to the reducer. Some common tasks that middleware perform include logging, caching, adding auth tokens to request headers, crash reporting, routing, and making asynchronous requests for data.

You can add any of these functionalities to your apps by using popular open-source middleware. Of course, you can also write your own middleware to solve problems that are specific to your application and its architecture.

`redux-thunk` - a middleware for handling asynchronous requests (it is included with Redux Toolkit by default).

<br>

## Write Your Own Middleware

Middleware runs after an action is dispatched and before that action is passed along to the reducer.

To add a middleware to our project, we use Redux’s `applyMiddleware` function like so.

```js
import { createStore, applyMiddleware } from "redux";
import { middleware1, middleware2, middleware3 } from "./exampleMiddlewares";
import { exampleReducer } from "./exampleReducer";
import { initialState } from "./initialState";

const store = createStore(
  exampleReducer,
  initialState,
  applyMiddleware(middleware1, middleware2, middleware3)
);
```

Once middleware has been added to a Redux project, calls to dispatch are actually calls to the middleware pipeline (the chain of all added middlewares). This means that any actions we dispatch will be passed from middleware to middleware before they hit an app’s reducers.

Middlewares must conform to a specific, nested function structure in order to work as part of the pipeline. That structure looks like this:

```js
const exampleMiddleware = (storeAPI) => (next) => (action) => {
  // do stuff here
  console.log(storeAPI.getState());

  const nextState = next(action);
  console.log(nextState);

  return nextState; // pass the action on to the next middleware in the pipeline
};
```

Each middleware has access to the `storeAPI` (which consists of the `dispatch` and `getState` functions), as well as the `next` middleware in the pipeline, and the `action` that is to be dispatched.

The body of the middleware function performs the middleware’s specific task before calling the next middleware in the pipeline with the current action.

<br>

## Introduction to Thunks

One of the most flexible and popular ways to add asynchronous functionality to Redux involves using thunks.

A **thunk** is a higher-order function that wraps the computation we want to perform later.

For example, this `add()` function returns a thunk that will perform `x+y` when called.

```js
const add = (x, y) => {
  return () => {
    return x + y;
  };
};
```

Thunks are helpful because they allow us to bundle up bits of computation we want to delay into packages that can be passed around in code. Consider these two function calls, which rely on the `add()` function above:

```js
const delayedAddition = add(2, 2);
delayedAddition(); // => 4
```

Note that calling `add()` does not cause the addition to happen – it merely returns a function that will perform the addition when called. To perform the addition, we must call `delayedAddition()`.

<br>

## redux-thunk

1. Asynchronous logic returns promises, and `store.dispatch` expects to receive a plain object with a `type` property.
2. Asynchronous operations create side effects. Including them in our reducers would violate a core tenet of Redux, which is that reducers must be pure functions.

Redux recommends making code with side effects part of the action creation process. It would be great if we could write action creators that return thunks, which would handle our asynchronous operations, in addition to the plain objects we’ve returned from our action creators thus far.

`redux-thunk` is a middleware that lets you do exactly that.

`redux-thunk` makes it simple for you to write asynchronous logic that interacts with the store by allowing you to write action creators that return thunks instead of objects.

These thunks can perform asynchronous operations - [redux-thunk documentation](https://github.com/reduxjs/redux-thunk#motivation): “can be used to delay dispatching an action” (for example, until after an API response is received), or “to dispatch an action only if certain conditions are met”.

A simple counter whose reducer contains a single value, which is updated by a single reducer. Without redux-thunk we are limited to writing synchronous action creators like this one:

```js
const increment = () => {
  return {
    type: "counter/increment",
  };
};
```

When we call `dispatch(increment())`, the value in our store immediately increases.

With `redux-thunk` we can extends an app to accommodate asynchronous action creators, example:

```js
const incrementLater = async () => {
  setTimeout(() => {
    dispatch(increment());
  }, 1000);
};

const asyncIncrement = () => {
  return incrementLater;
};
```

redux-thunk is such a popular solution for handling asynchronous logic that it is included in Redux Toolkit. It also exists as a standalone package, but you won’t need to install redux-thunk separately if you use Redux Toolkit. This is because Redux Toolkit’s configureStore function, will apply redux-thunk to the store by default.

<br>

## Writing Thunks in Redux

Example: we need to fetch a user's data from an API. We would like to be able to dispatch an action creator that would first perform an asynchronous operation, (fetching the data), and then dispatch a synchronous action (adding the data to the store) after the asynchronous operation completes.

`redux-thunk` allow us to write action creators that return thunks, within which we can perform any asynchronous operations we like.

Asynchronous action creator:

```js
import { fetchUser } from "./api";

const getUser = (id) => {
  return async (dispatch, getState) => {
    const payload = await fetchUser(id);
    dispatch({ type: "users/addUser", payload: payload });
  };
};
```

`getUser` has two key parts: the synchronous outer function (otherwise known as the thunk action creator) which returns the inner, asynchronous thunk.

The thunk receives `dispatch` and `getState` as arguments, and dispatches a synchronous action after the asynchronous operation (`fetchUser`) completes.

To get the user with `id = 32`, we can call `dispatch(getUser(32))`. Note that the argument to `dispatch` is not an object, but an asynchronous function that will first fetch the user’s data and then dispatch a synchronous action once the user’s information has been retrieved.

<br>

## redux-thunk Source Code

The `redux-thunk` middleware performs a simple check to the argument passed to `dispatch`. If `dispatch` receives a function, the middleware invokes it; if it receives a plain object, then it passes that action along to reducers to trigger state updates.

Source Code:

[Project's Repo](https://github.com/reduxjs/redux-thunk/blob/master/src/index.ts)

```js
function createThunkMiddleware(extraArgument) {
  return ({ dispatch, getState }) =>
    (next) =>
    (action) => {
      if (typeof action === "function") {
        return action(dispatch, getState, extraArgument);
      }

      return next(action);
    };
}

const thunk = createThunkMiddleware();
thunk.withExtraArgument = createThunkMiddleware;

export default thunk;
```

Recall getUser, the thunk action creator from the previous exercise:

```js
const getUser = (id) => {
  return async (dispatch, getState) => {
    const payload = await fetchUser(id);
    dispatch({ type: "users/addUser", payload: payload });
  };
};
```

The key step happens where the middleware checks whether or not the action is a function. If the action is a function (as happens when we dispatch thunks returned by thunk action creators), then redux-thunk invokes that function. If the action is not a function (as happens when we dispatch plain objects), redux-thunk passes it through to the next step in the middleware pipeline.

Suppose we were to call dispatch(getUser(7)) with the thunk middleware applied. We know that getUser(7) returns a thunk, so on line 3 of the thunk middleware, typeof action === 'function' will evaluate to true. On line 4, the middleware will then invoke getUser(7) with the arguments dispatch and getState. This invocation will initiate the asynchronous fetching performed by the thunk. When that asynchronous fetching is complete, the thunk will dispatch the synchronous action {type: ‘users/addUser’, payload: payload}.

For contrast, let’s walk through what happens when we dispatch that synchronous action. Since the action is a plain object, typeof action === 'function' will evaluate to false. The redux-thunk middleware therefore skips to line 7, and invokes the next middleware in the pipeline, passing the action along.

<br>

## Promise Lifecycle Actions

We need to keep track of the state our async requests are in at any given moment so that we can reflect those states for the user.

It is common to dispatch a “pending” action right before performing an asynchronous operation, and “fulfilled” or “rejected” actions depending on the results of the completed operation.

Thunk that include pending and rejected actions:

```js
import { fetchUser } from "./api";
const fetchUserById = (id) => {
  return async (dispatch, getState) => {
    dispatch({ type: "users/requestPending" });
    try {
      const payload = await fetchUser(id);
      dispatch({ type: "users/addUser", payload: payload });
    } catch (err) {
      dispatch({ type: "users/error", payload: err });
    }
  };
};
```

We call these pending/fulfilled/rejected actions _promise lifecycle actions_.

This pattern is so common that Redux Toolkit provides a neat abstraction, `createAsyncThunk`, for including promise lifecycle actions in your Redux apps

<br>

## createAsyncThunk()

```js
import { createAsyncThunk } from "@reduxjs/toolkit";
```
