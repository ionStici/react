## Components, Instances, Elements

**Components:** are JS functions that return JSX.

**An Instance:** is a reference to a component. Each time a component is used, React creates a new instance of that component, which includes its own state and lifecycle.

**An Element:** (or JSX) is a built-in component (object) that describes a DOM node. Each JSX element is converted to a `React.createElement()` call.

**Note:** React calls the component instances internally when it renders.

<br>

## Rules for Render Logic: Pure Components

**Render Logic** - Code that lives at the top level of the component function; Participates in describing how the component view looks like; Code executed every time the component renders;

**Event Handler Functions** - Code executed as a consequence of the event that the handler is listening for; Code that does things: update state, perform an HTTP request, read an input field, navigate to another page, etc.

**Side effects** - modification of any data outside the function score (mutating external variables, HTTP requests, writing to DOM);

**Pure functions** - a function that has **no** side effects; Functions that do not change any variable outside their scope; Given the same input, a pure function always returns the same output;

### Rules for Render Logic

1. Components must be pure when it comes to render logic: given the same props (input), a component instance should always return the same JSX (output).

2. Render logic must produce no side effects: no interaction with the "outside world" is allowed. So, in render logic:

   - Do NOT perform API calls
   - Do NOT start timers
   - Do NOT directly use the DOM API
   - Do NOT mutate objects or variables outside of the function scope
   - Do NOT update state (or refs): this will create an infinite loop

These side effects, are only forbidden inside render logic. We have other options for running side effects: Side effects are allowed (and encouraged) in event handler functions (which are not part of the render logic). These is also a special hook to register side effects `useEffect`.

<br>

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

## How Events Work in React

Event Delegation works in React as well.

When we register an event handler on a JSX element, for example using the `onClick` prop, behind the scenes React actually registers only one event handler per event type at the root node of the fiber tree during the render phase.

If we have multiple `onClick` handlers in our code, React will somehow bundle them all together and just add one big `onClick` handler to the `#root` node of the fiber tree.

Behind the scenes, React performs event delegation for all events in our application.

React delegates all events to the `#root` DOM container, where they will get handled, not in the place where we attach the `onClick` prop.

### Synthetic Events

When we declare and event handler like this one:

```jsx
<input onChange={(e) => setText(e.target.value)} />
```

..react gives us access to the event object that was created, just like in vanilla JS. However, in React, this event object is different.

In vanilla JS we get access to the native DOM event object, React on the other hand will give us something called a synthetic event, which is a thin wrapper around the DOM's native event object.

By wrapper, we mean that synthetic events are similar to native event objects, but they add or change some functionalities on top of them.

These synthetic events have the same interface as native event objects, including important methods like `preventDefault`. What's special about synthetic events thought, and one of the reasons why the React team decided to implement them is the fact that they fix some browser incosistencies, so that events work in the exact same way in all browsers. Most synthetic events actually bubble (including focus, blur, and change which usually do not bubble), except for scroll.
