## Presentational Components and Container Components

[Presentational and Container Components by Dan Abramov](https://medium.com/@dan_abramov/smart-and-dumb-components-7ca2f9a7c7d0)

_React programming pattern:_ **Separating Container Components from Presentational Components.**

- **Presentational Component:** its only job is to contain JSX, and it will be rendered by a container component.
- **A Container Component:** will handle the app logic like fetching data and dealing with state, then rendering the _presentational components_ (instead of its own JSX).

```js
// Directory Structure

components/  // folder for presentational components
  |-- Presentational.js  // component that only contains JSX, but not render anything

containers/  // folder for handling logic
  |-- Container.js  // will render the Presentational.js
```

_Example:_ `Container.js` will import and render the `Presentational.js`.

<br>

## Sibling to Sibling Communication

How to communicate between a presentational (stateless) component and a container (stateful) component, so that it will trigger screen rendering.

```jsx
function Container() {
  const [active, setActive] = useState(false);

  return <Presentational active={active} toggle={setActive} />;
}

function Presentational(props) {
  return <button onClick={() => props.toggle(!props.active)}>Click</button>;
}
```

When `Presentational` needs to communicate a change, it uses the function passed to it through the `toggle` prop.

<br>
