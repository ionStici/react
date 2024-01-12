# Props

A component can pass data "props" to another inner component.

`props` is the name of the object that stores the passed data.

To pass `props`, we simply define key / value pairs

inside the component's angle brackets.

```jsx
function App() {
  return <User name="John" logIn={true} nums={[7, 3]} />;
}

function User(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

To access `props` inside a component's body, first we specify the `props` parameter, then we access the data using the pattern: `props.propertyName`.

`props` are read-only, they are **immutable**. If you need to mutate props, then you should use **state**.

React uses a one-way data flow approach, which means that data can only be passed from parent components to child components, and not the other way around.

<br>

## Event Handler as Props

```jsx
function Button(props) {
  return <button onClick={props.onClick}>Click</button>;
}

function App() {
  const handleClick = () => console.log("click");
  return <Button onClick={handleClick} />;
}
```

We provide an event function to a child component as a prop.

Through the `props` object, the child component will use the provided prop as an event handler.

- **Naming Convention**

  _Event Functions:_ keyword `handle` + the type of the event `Click`

  _Props:_ keyword `on` + the type of the event `Click`

<br>

## Destructuring Props

```jsx
function Header({ userData }) {}
```

<br>

## props.children

The `props` object has a `children` property which will return everything we pass between a component's opening and closing JSX tags.

```js
function Layout(props) {
  return <main>{props.children}</main>;
}

function App() {
  return <Layout>Hello</Layout>;
}
```

If the component is an self-closing tag, then `children` will be `undefined`.

<br>

## defaultProps

Define default props using the `defaultProps` property.

```jsx
Comp.defaultProps = {
  property: "value",
};
```

We append the `defaultProps` property on the component name and set equal to an object.

Inside this object, we define default properties for any prop we want.

<br>
