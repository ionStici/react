# Draft

<!-- The react code is processed in the build step, this means that the code we write will not be the code that ends up in the browser, we simply write code that is convenient for us the developer, then behind the scenes that code will be transformed before it reaches the browser.

`index.js` file is the starting point of the react application. In here, all the react code is imported. After which, the `render` method will insert all these code in a single html document right inside the `div` element with the `root` id, but only after the build step. -->

<br>
<br>
<br>

# How React Works

## Components, Instances, and Elements

- **Components** are JS functions that returns JSX.

- **An Instance** is a reference to a component. Each time a component is used, React creates a new instance of that component, which includes its own state and lifecycle.

- **An Element** (or JSX) is a built-in component (object) that describes a DOM node. Each JSX element is converted to a `React.createElement()` call.

**Note:** React calls the component instances internally when it renders.

<br>

## How Rendering Works

### 1. Render is Triggered

_..by updating state somewhere_

- The 2 situations that trigger renders: **(1) Initial render** of the application, **(2) State is updated** in a component instance (**re-render**)

- Internally, the render process is triggered for the entire application, but the DOM is updated by React only where the update happened

### 2. Render Phase

2. **Render Phase** - react calls component functions and figures out how DOM should be updated, this happens internally inside React and does not produce visual changes yet

   At the beginning of the render phase, React will go through the entire component tree, take all the component instances that triggered a re-render and render them, which means to call the corresponding component functions. This will create updated React elements which altogether make up the so-called virtual DOM.

   React constructs a Virtual DOM, which is tree of all the React elements created from all instances in the component tree.

   Rendering a component will cause all of its child components to be rendered as well, necessary because React doesn't know whether children will be affected.

   The new Virtual DOM that was created after the state update will get reconciled with the current Fiber Tree as it exists before the state update. This reconciliation is done in React's reconciler which is called "Fiber".

   The result of this reconciliation process is gonna be an updated Fiber Tree, a tree which will eventually be used to write to the DOM.

#### Reconciliation

Why not update the entire DOM whenever state changes somewhere in the app? Because that would be inefficient in terms of performance. Usually only a small part of the DOM needs to be updated.

React reuses as much of the existing DOM as possible. How? **Reconciliation**: Deciding which DOM elements actually need to be inserted, deleted, or updated in order to reflect the latest state changes.

The result of the rconciliation process is gonna be a list of DOM operations that are necessary to update the current DOM with a new state.

Reconciliation is processed by a **reconciler**, we can say that the reconciler is the engine of React. It's this reconciler that allows us to never touch the DOM directly, and instead simply tell React what the next snapshot of the UI should look like based on state.

The current reconciler in React is called **Fiber**.

#### Fiber Reconciler

During the initial render of the app, Fiber takes the entire React element tree (the virtual DOM), and based on it builds yet another tree which is the **Fiber tree**.

The fiber tree is a special internal tree where for each component instance and DOM element in the app there is one so-called **Fiber**.

Unlike react elements in the virtual DOM, Fiber are not recreated on every render. So the fiber tree is never destroyed. Instead, it's a mutable data structure, and once it has been created during the initial render, it's simply mutated over and over again in future reconciliation steps.

The actual state and props of any component instance are internally stored inside the corresponding fiber in the fiber tree. Each fiber also contains a queue of work to do, like updaitng state, updating refs, running registered side effects, performing DOM updates and so on. This is why a fiber is also defined as a unit or work.

In the fiber tree, the fibers are arranged in a linked list way, it makes it easier for React to process the work that is associated with each Fiber.

Considering that each fiber is a unit of work, this work can be done asynchronously, this means that the rendering process, which is what the reconciler does, can be split into chunks, some tasks can be prioritized over others, and work can be paused, reused, or thrown away if not valid anymore. All this enables concurrent features like suspense or transitions staritng in React 18, it also allows the rendering process to pause and resume later so that it won't block the browser's JS engine with too long renders.

#### The Reconciliation Process in Action

The result of the Reconciliation process + Diffing is a second updated fiber tree plus a list of DOM updates that need to be performed in the next phase. React still hasn't written anything to the DOM yet, but it has figured out this so-called list of effects.

### 3. Commit Phase

React writes to the DOM, updating inserting, deleting elements

4. **Browser Paint**

<br>
