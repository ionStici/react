# Effects and Data fetching

## Table of Content

- [The Component Instance Lifecycle](#the-component-instance-lifecycle)
- [Side Effects](#side-effects)
- [The useEffect Hook](#the-useeffect-hook)
- [The Dependency Array](#the-dependency-array)

<br>

## The Component Instance Lifecycle

The lifecycle of a component instance encompasses 3 phases:

1. **Mount / Initial Render**

2. **Re-render** - Happens when: state change, prop change, parent re-render, context change.

3. **Unmount** - the component instance is destroyed.

We can execute code in these different phases of a component's lifecycle using the `useEffect` hook.

<br>

## Side Effects

A **side effect** is any interaction between a react component and the world outside that component. Examples: data fetching, timers, accessing the DOM, etc.

**Note:** Side effects should not be defined in the render logic.

We can create side effects in 2 different places in React: inside **Event Handlers** and using the **`useEffect` hook**.

<br>

## The useEffect Hook

```jsx
import { useState, useEffect } from "react";

function App() {
  const [data, setData] = useState(null);

  useEffect(() => {
    async function fetchData() {
      const res = await fetch();
      const data = await res.json();
      setState(data);
    }

    fetchData();
  }, []);

  return <p>JSX</p>;
}
```

- Setting state in the render logic will cause the component to re-render itself again and again indefinitely. The solution to this is to set state inside the `useEffect` hook.

- The `useEffect` hook is composed of 3 parts: the effect code, the dependency array, a cleanup function that must be returned.

- The first argument of `useEffect` is a callback (nicknamed "effect") that should contain the side effect code.

- The second argument of `useEffect` is the dependency array.

<br>

## The Dependency Array

`useEffect` is like an event listener that is listening for its dependencies to change. Whenever a dependency changes, the effect is executed again.

Dependencies are always state or props, and when state or props changes the component re-renders, this means that effects and the lifecycle of components are interconnected.

```jsx
// Different Scenarios
useEffect(runEffect);
useEffect(runEffect, []);
useEffect(runEffect, [x, y, z]);
```

- **Multiple dependencies:** the effect runs on the initial render and on each re-render triggered by updating one of the dependencies, it synchronizes with these dependencies. If other state or prop is updated then this particular effect will not be executed.

- **An empty dependency array:** the effect runs only on mount, no synchronization.

- **No array at all:** the effect runs on every render, it synchronizes with everything.

### When are effects executed?

The component instance mounts, the result of rendering is committed to the DOM, the DOM changes are painted onto the screen.

Effects are only executed after the browser has painted the component instance on the screen, and not immediately after render. Why? Because effects may contain long-running processes such as fetching data.

<br>
<br>
<br>

## The useEffect Hook

- The idea of the `useEffect` hook is to give us a place where we can safely write side effects like data fetching.

- Executed after the component mounts (initial render), and after subsequent re-renders (according to dependency array).

- The cleanup function will be called before the component re-renders or unmounts.

<br>

### When are effects executed?

One important consequence of the fact that effects do not run during render is that if an effect sets state, then a second additional render will be required to display the UI correctly, because of this you should not overuse effects. In other words: if an effect sets state, an **additional render** will be required.

Again, code inside `useEffect` is executed after all the render logic code runs, so all the code that a component has.

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## The useEffect Cleanup Function

```jsx
useEffect(() => {
  // code ...

  return () => "";
}, []);
```

`useEffect` cleanup function

- Function that we can return from an effect (optional)

- Runs on two different occasions:

  1. Before the effect is **executed again** (in order to cleanup the results of the previous side effect)
  2. After a component has **unmounted**, (in order to give us the opportunity to reset the side effect that we created if that's necessary)

<br>

- Component renders -> Execute effect if dependency array includes updated data
- Component unmounts -> Execute cleanup function

When we need a cleanup function? Whenever the side effect keeps happening after the component has been re-rendered un unmounted. Examples: start timer -> stop timer, add event listener -> remove listener, etc.

Each effect should do **only one thing.** Use one `useEffect` hook for each side effect. This makes effects easier to clean up.

If you need to create multiple effects, just use multiple useEffect hooks.

The cleanup function runs after the component has already unmounted, even so thanks to JS closures the cleanup function will remember the previous variables.

<br>

## Cleaning Up Data Fetching

_Race condition_

```jsx
useEffect(() => {
  const constroller = new AbortController();

  async function fetchData() {
    const res = fetch("apilink", { signal: controller.signal });
  }

  return () => controller.abort();
}, [dep]);
```

## Listening to a Keypress

Event listeners should be placed inside `useEffect` hook, because they are side effects.

Each time the same component mounts, a new event listener is added to the document, always an additional one to the ones that we already have, so we have to cleanup our event listeners.

```jsx
useEffect(() => {
  const callback = () => "";

  document.addEventListener("keydown", callback);

  return () => document.removeAddEventListenr("keydown", callback);
}, []);
```

<br>
<br>
<br>
<br>
<br>

## Function Component Effects

Importing the Effect Hook from the `react` library:

```js
import { useEffect } from "react";
```

The Effect Hook is used to call a callback that does something for us (nothing is returned).

`useEffect()` receives a callback function (nicknamed _effect_) that React calls each time the component renders.

```js
function Comp() {
  const [name, setName] = useState("");

  useEffect(() => {
    document.title = `Hi, ${name}`;
  });
}
```

Notice how we use the current state inside of our effect. Even though our effect is called after the component renders, we still have access to the variables in the scope of our function component! When React renders our component, it will update the DOM as usual, and then run our effect after the DOM has been updated. This happens for every render, including the first and last one.

<br>

## Clean Up Effects

Node: When we add event listeners to the DOM, it is important to remove those event listeners when we are done with them to avoid memory leaks!

Because effects run after every render and not just once, React calls our cleanup function before each re-render and before unmounting to clean up each effect call.

```js
useEffect(() => {
  btn.addEventListener("click", handleClick);

  return () => {
    btn.removeEventListener("click", handleClick);
  };
});
```

If our effect returns a function, then the `useEffect()` Hook always treats that as a cleanup function. React will call this cleanup function before the component re-renders or unmounts. Since this cleanup function is optional, it is our responsibility to return a cleanup function from our effect when our effect code could create memory leaks.

<br>

## Control When Effects are Called

The useEffect() function calls its first argument (the effect) after each time a component renders.

How to skip effect calls?

If we want to only call our effect after the first render, we pass an empty array to `useEffect()` as the second argument. this second argument is called the _dependency array_.

The dependency array is used to tell the `useEffect()` method when to call our effect and when to skip it.

Our effect is always called after the first render but only called again if something in our dependency array has changed values between renders.

```js
useEffect(() => {
  status = true;
  return () => (status = false);
}, []);
```

If a cleanup function is returned by our effect, it will be called when the component unmounts.

Passing [] to the useEffect() function is enough to configure when the effect and cleanup functions are called!

<br>

## Fetch Data

The default behavior of the Effect Hook is to call the effect function after every single render.

We can pass an empty array as the second argument for useEffect() if we only want our effect to be called after the component’s first render.

How to use the dependency array to configure exactly when we want our effect to be called.

When the data that our components need to render doesn’t change, we can pass an empty dependency array, so that the data is fetched after the first render. When the response is received from the server, we can use a state setter from the State Hook to store the data from the server’s response in our local component state for future renders. Using the State Hook and the Effect Hook together in this way is a powerful pattern that saves our components from unnecessarily fetching new data after every render!

An empty dependency array signals to the Effect Hook that our effect never needs to be re-run, that it doesn’t depend on anything. Specifying zero dependencies means that the result of running that effect won’t change and calling our effect once is enough.

A dependency array that is not empty signals to the Effect Hook that it can skip calling our effect after re-renders unless the value of one of the variables in our dependency array has changed. If the value of a dependency has changed, then the Effect Hook will call our effect again!

```js
useEffect(() => {
  document.title = "Hello";
}, [count]);
```

Only re-run the effect if `count` changes

[React Docs Resource](https://reactjs.org/docs/hooks-effect.html#tip-optimizing-performance-by-skipping-effects)

<br>

## Rules of Hooks

Two main rules when using Hooks:

- Only call Hooks at the top level
- Only call Hooks from React functions

[Hooks API Reference](https://reactjs.org/docs/hooks-reference.html)

When React builds the Virtual DOM, the library calls the functions that define our components over and over again as the user interacts with the user interface. React keeps track of the data and functions that we are managing with Hooks based on their order in the function component’s definition. For this reason, we always call our Hooks at the top level; we never call hooks inside of loops, conditions, or nested functions.

Secondly, Hooks can only be used in React Functions. We cannot use Hooks in class components and we cannot use Hooks in regular JavaScript functions. We’ve been working with useState() and useEffect() in function components, and this is the most common use. The only other place where Hooks can be used is within custom hooks. Custom Hooks are incredibly useful for organizing and reusing stateful logic between function components. For more on this topic, head to the [React Docs](https://reactjs.org/docs/hooks-custom.html).

<br>
