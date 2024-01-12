[&larr; Back](./README.md)

# The Effect Hook

Three key moments when the Effect Hook can be utilized:

1. When the component is first mounted to the DOM and renders
2. When the state or props change, causing the component to re-render
3. When the component is unmounted from the DOM

<br>

## Table of Content

- [Function Component Effects](#function-component-effects)
- [Clean Up Effects](#clean-up-effects)
- [Control When Effects are Called](#control-when-effects-are-called)
- [Fetch Data](#fetch-data)
- [Rules of Hooks](#rules-of-hooks)
- [Separate Hooks for Separate Effects](#separate-hooks-for-separate-effects)

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

## Separate Hooks for Separate Effects

```js
function Comp() {
  const [status, setStatus] = useState(true);

  useEffect(() => {
    // ...
  });

  useEffect(() => {
    // ...
  });
}
```

[Should I use one or many state variables?](https://reactjs.org/docs/hooks-faq.html#should-i-use-one-or-many-state-variables)

<br>
