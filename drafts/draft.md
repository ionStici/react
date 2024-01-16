# Draft

<!-- The react code is processed in the build step, this means that the code we write will not be the code that ends up in the browser, we simply write code that is convenient for us the developer, then behind the scenes that code will be transformed before it reaches the browser.

`index.js` file is the starting point of the react application. In here, all the react code is imported. After which, the `render` method will insert all these code in a single html document right inside the `div` element with the `root` id, but only after the build step. -->

# How React Works

## Components, Instances, and Elements

- **Components** are JS functions that returns JSX.

- **An Instance** is a reference to a component. Each time a component is used, React creates a new instance of that component, which includes its own state and lifecycle.

- **An Element** (or JSX) is a built-in component (object) that describes a DOM node. Each JSX element is converted to a `React.createElement()` call.

**Note:** React calls the component instances internally when it renders.

## How Rendering Works

Component A => Component Instance A1 => React Element => DOM Element (HTML) => User Interface on the screen.

1. **Render is Triggered** - by updating state somewhere

   - The 2 situations that trigger renders: **(1) Initial render** of the application, **(2) State is updated** in a component instance (**re-render**)

   - Internally, the render process is triggered for the entire application, but the DOM is updated by React only where the update happened

2. **Render Phase** - react calls component functions and figures out how DOM should be updated, this happens internally inside React and does not produce visual changes yet

3. **Commit Phase** - React writes to the DOM, updating inserting, deleting elements

4. **Browser Paint**
