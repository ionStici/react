# Thinking in React

## State Management

**Local State:** State needed only by one or few components. State that is defined in a component and only that component and child components have access to it (by passing via props).

**Global State:** State that many components might need. Shared state that is accessible to every component in the entire application. Tools for managing global state: Context API & Redux.

**Derived State:** state that is computed from an existing piece of state or from props.

<br>

## Lifting State Up

**Lifting State Up:** whenever multiple sibling components need access to the same state, we move that piece of state up to the first common parent component.

```jsx
function App() {
  const [items, setItems] = useState([]);

  const handleAddItems = (item) => {
    setItems((prev) => [...prev, item]);
  };

  return (
    <>
      <Form onAddItems={handleAddItems} />
      <Items items={items} />
    </>
  );
}

function Form({ onAddItems }) {
  const input = useRef(null);

  const handleClick = () => {
    onAddItems(input.current.value);
  };

  return (
    <>
      <input type="text" ref={input} />
      <button>Click</button>
    </>
  );
}

function Items({ items }) {
  return <p>{items.join(" ")}</p>;
}
```

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

function Child(props) {
  return <h1>{props.name}</h1>;
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

## Child Components Update Sibling Components

A child component updates its parent’s state, and the parent passes that state to a sibling component.

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

<br>
