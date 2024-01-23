# React

[**React**](https://react.dev/) is a declarative JavaScript library for building reactive UIs.

Why React? Fast, modular, scalable, flexible, popular.

## Table of Content

- [JSX](#jsx)
- [Babel Transformation](#babel-transformation)
- [Nested JSX and Outer Elements](#nested-jsx-and-outer-elements)
- [className](#classname)
- [htmlFor](#htmlfor)
- [Self-Closing Tags](#self-closing-tags)
- [Curly Braces in JSX](#curly-braces-in-jsx)
- [Event Listeners in JSX](#event-listeners-in-jsx)
- [JSX and Conditionals](#jsx-and-conditionals)
- [`map` in JSX](#map-in-jsx)

<br>

## JSX

**JSX** (JavaScript XML) is a syntax extension for JavaScript, that allows developers to declaratively write html elements directly within JavaScript code.

```jsx
<h1>Hello</h1> // JSX element
```

JSX elements are treated as JS expressions, which means that we can store them in variables.

```jsx
const h1 = <h1 id="h1">Hello</h1>;
```

JSX elements accept html attributes just like html elements do.

_p.s. We can think of JSX elements as built-in react components._

<br>

## Babel Transformation

Before reaching the browser, JSX code is compiled into regular JavaScript objects that create HTML elements on the DOM.

This piece of JSX code:

```jsx
const element = <h1>Hello, World!</h1>;
```

is transformed by Babel into this:

```js
// JS
const element = React.createElement("h1", null, "Hello, World!");
```

<br>

## Nested JSX and Outer Elements

```JSX
const list = (
    <ul>
        <li>JSX</li>
        <li>React</li>
    </ul>
);
```

**JSX rules:**

1. If a JSX expression takes up more than one line, then you must wrap it in parentheses.
2. Any JSX expression must have exactly one outer (parent) element.

### React Fragment

Use the `<Fragment>` react component (or the short `<>`) as the root element in case you need more outer elements.

<br>

## className

To set a `class` attribute in JSX, we use the `className` keyword instead.

```jsx
<h1 className="heading-1">Hello</h1>
```

<br>

## htmlFor

In JSX, the `for` attribute is written as `htmlFor`.

```jsx
<label htmlFor="name">Name</label>
```

<br>

## Self-Closing Tags

In JSX, for self-closing tags, it is mandatory to include a forward-slash before the final angle-bracket.

```JSX
<img src="/img.jpg" />
```

<br>

## Curly Braces in JSX

Code within curly braces inside a JSX expression will be treated as JS code.

They mark the beginning and the end of a JS injection into JSX.

```jsx
const myName = "John";
const hello = <p>Hello, {myName}</p>;
```

We can place JS expressions inside JSX curly braces, but not statements.

<br>

## Event Listeners in JSX

```jsx
function App() {
  const handleClick = () => console.log("hi");
  return <button onClick={handleClick}>Click</button>;
}
```

- Event listeners are defined on JSX elements by special attributes: [Supported Events](https://reactjs.org/docs/events.html#supported-events).

- The name of the event listener attribute: The keyword `on` + the event type (in camelCase).

- The value of the event listener attribute should be a predefined function.

<br>

## JSX and Conditionals

_Conditional rendering_

- JSX and **`if` statements** (note: we can't inject `if` into JSX)

  ```jsx
  let myName;
  if (true) myName = <p>John</p>;
  ```

- **Ternary Operator** (inside JSX)

  ```jsx
  const p = <p>{true ? "first" : "second"}</p>;
  ```

- **`&&`**

  ```jsx
  const p = {true && <p>Hello</p>};
  ```

<br>

## `map` in JSX

```jsx
const arr = ["dog", "note", "cactus"];
const list = arr.map((item) => <li key={item}>{item}</li>);
```

Using `map` we can dynamically render JSX elements.

### The key attribute

`key` is a special attribute, its value should be something unique.

React uses `key` internally to keep track of lists rendered using the `map` method.

<br>
