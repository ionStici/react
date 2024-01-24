# Thinking in React

## Table of Content

- [State Management](#state-management)
- [Stateless Components Inherit from Stateful Components](#stateless-components-inherit-from-stateful-components)
- [Child Components Update Their Parents' state](#child-components-update-their-parents-state)
- [Child Components Update Their Sibling Components](#child-components-update-their-sibling-components)
- [Component Categories]()

<br>

## State Management

**Local State:** State needed only by one or few components. State that is defined in a component and only that component and child components have access to it (by passing via props).

**Global State:** State that many components might need. Shared state that is accessible to every component in the entire application. Tools for managing global state: Context API & Redux.

**Derived State:** state that is computed from an existing piece of state or from props.

<br>

## Stateless Components Inherit from Stateful Components

- “Stateful” describes any component that has a state property;
- “Stateless” describes any component that does not.

A child component can access the state of its parent component through props.

```jsx
function Parent() {
  const [name, setName] = useState("John");
  return <Child name={name} />;
}

function Child({ name }) {
  return <h1>{name}</h1>;
}
```

- `props` for storing information that can be changed only by a different component.
- `state` for storing information that the component itself can change.

<br>

## Child Components Update Their Parents' state

```jsx
function Parent() {
  const [toggle, setToggle] = useState(true);
  const changeState = () => setToggle((prev) => !prev);

  return <Child toggle={toggle} onClick={changeState} />;
}

function Child({ toggle, onClick }) {
  return <button onClick={onClick}>{toggle + ""}</button>;
}
```

<br>

## Child Components Update Their Sibling Components

**Lifting State Up:** whenever multiple sibling components need access to the same state, we move that piece of state up to the first common parent component.

_Concept:_ one stateless component displays information, and a different stateless component offer the ability to change that information.

```jsx
const ChildOne = (props) => <button onClick={props.onClick}>Click</button>;
const ChildTwo = (props) => <p>{props.text}</p>;

function Parent() {
  const [text, setText] = useState("Hello");
  const changeState = () => setText("Hello World");

  return (
    <>
      <ChildOne onClick={changeState} />
      <childTwo text={text} />
    </>
  );
}
```

A child component updates its parent’s state, and the parent passes that state to a sibling component.

<br>

## Component Categories

- **Stateless / Presentational Components**

  - No state
  - Receive props
  - Simply present data

- **Stateful Components**

  - Have state

- **Structural Components**

  - Pages, layouts of the app, provides structure
  - Result of component composition

### Component Composition

**Prop Drilling** occurs when a parent component passes data down to its children and then those children pass the same data down to their own children.

**Component Composition** - technique of combining different components using the `children` prop (or explicitly defined props) / for reusable and flexible components / to fix a prop drilling problem / great for creating layouts.

```jsx
// Component Composition Example
function App() {
  return (
    <Layout>
      <NavBar>
        <Logo />
      </NavBar>

      <List>
        <Box />
        <Box />
      </List>
    </Layout>
  );
}
```
