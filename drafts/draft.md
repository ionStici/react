# Draft

## Component Categories

- Stateless / Presentational Components

  No state

  Receive props

  Simply present data

- Stateful Components

  Have state

- Structural Components

  Pages, layouts of the app, Provides structure

  Result of composition of components

**Prop Drilling** occurs when a parent component passes data down to its children and then those children pass the same data down to their own children.

**Component Composition** - technique of combining different components using the `children` prop (or explicitly defined props) / for reusable and flexible components / to fix a prop drilling problem / great for creating layouts

```jsx
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
```

## Explicit Prop

Passing Components as Props (Alternative to children)

```jsx
function App() {
  return <Layout element={<Component />} />;
}

function Layout({ element }) {
  return <main>{element}</main>;
}
```

# How React Works

## Components, Instances, and Elements

**Components** - is a function that returns React elements.

**Component Instances** - are created when we use the component. Each instance holds its own state and props and has its own life cycle.

**React Elements** - are the result of calling a component function containing JSX which eventually is converted to DOM elements.

**DOM Element (HTML)** - the actual visual representation of the component instance in the browser.

```jsx
function App() {
  return (
    <div>
      <Card /> // Component Instance
    </div>
  );
}

// Component
function Card() {
  return <p>Hello</p>; // React Element
}
```

## How Rendering Works
