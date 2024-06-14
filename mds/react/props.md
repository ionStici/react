# Props

## Table of Contents

- [Passing data with props](#passing-data-with-props)
- [Event Handlers as Props](#event-handlers-as-props)
- [props.children](#propschildren)
- [Explicit Props](#explicit-props)
- [Component Composition](#component-composition)
- [defaultProps](#defaultprops)

## Passing data with props

In React, we use "props" to pass data from parent components to child components.

Props are defined as key/value pairs within the opening tag of the component.

```jsx
function App() {
  return <User name="John" logIn={true} nums={[7, 3]} />;
}

function User(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

To access `props` within a component, we specify the `props` parameter in the component's function definition. The props can then be accessed as properties of this `props` object. For instance, `props.name` refers to the `name` prop passed to the `User` component.

**Key points about props:**

1. **Immutability:** _Props are read-only._ They cannot be modified within the component that receives them. If you need to modify the data, consider using state instead.

2. **One-Way Data Flow:** React follows an unidirectional data flow pattern. This means that props can only be passed from parent components to child components, not in reverse.

3. **Destructuring Props:** For convenience and readability, its common to use destructuring to directly extract specific properties from the `props` object.

```jsx
function Header({ userData }) {}
```

## Event Handlers as Props

In React, we often need to pass event handlers from a parent component to a child component. This is commonly done using props.

```jsx
function Button({ onClick }) {
  return <button onClick={onClick}>Click</button>;
}

function App() {
  const handleClick = () => console.log('click');
  return <Button onClick={handleClick} />;
}
```

`App` passes the `handleClick` function to the `Button` component as a prop named `onClick`. The `Button` child component then uses this prop as an event handler for its click event.

**Naming Convention**

- _Event Functions:_ prefix `handle` + the event type `Click`
- _Props:_ prefix `on` + the event type `Click`

## props.children

`props.children` is a special prop, automatically passed to every component, that renders whatever is included between the component's opening and closing JSX tags.

```js
function Layout(props) {
  return <main>{props.children}</main>;
}

function App() {
  return <Layout>Hello</Layout>;
}
```

In this code example, `Layout` will render the string `Hello`. If the component is self-closing, then `children` will be `undefined.`

## Explicit Props

We can explicitly pass components or elements as props.

```jsx
function App() {
  return <Layout element={<Component />} />;
}

function Layout({ element }) {
  return <main>{element}</main>;
}
```

This approach offers more flexibility and control over where and how the passed elements are rendered within the `Layout` component.

### Component Composition

**Component Composition** is a technique of combining different components using the `children` props (or explicitly defined props), making it easier to build complex UIs and avoiding the prop drilling problem.

```jsx
function App() {
  return (
    <Layout>
      <Header>
        <Logo />
        <Navigation />
      </Header>
      <MainContent>
        <Item />
        <Item />
      </MainContent>
    </Layout>
  );
}
```

Prop drilling occurs when a prop needs to be passed through multiple levels of nested components that do not use the prop themselves, but are merely intermediaries to pass the prop to a deeply nested child component that actually needs it.

## defaultProps

Define default values for your component's props using `defaultProps`.

```jsx
function Nav({ property }) {
  return <p>{property}</p>;
}

Nav.defaultProps = {
  property: 'value',
};
```
