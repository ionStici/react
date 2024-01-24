# How React Works

## Table of Content

- [Components, Instances, Elements](#components-instances-elements)
- [How Rendering Works](#how-rendering-works)

<br>

## Components, Instances, Elements

**Components:** are JS functions that return JSX.

**An Instance:** is a reference to a component. Each time a component is used, React creates a new instance of that component, which includes its own state and lifecycle.

**An Element:** (or JSX) is a built-in component (object) that describes a DOM node. Each JSX element is converted to a `React.createElement()` call.

**Note:** React calls the component instances internally when it renders.

<br>

## How Rendering Works

### 1. Render is Triggered

_..by updating state somewhere_

Two situations that trigger renders: **(1) Initial render** of the application, **(2) State is updated** in a component instance **(re-render)**

Internally, the render process is triggered for the entire application, but the DOM is updated by React only where the updated happened.

### 2. Render Phase

The **Render Phase** in React involves determining how the DOM should be updated. This internal process within React does not produce visual changes immediately.

- **Component Rendering:** React traverses the entire component tree. It renders components that have triggered a re-render by calling their component functions. This generates updated React elements, forming a new Virtual DOM.

- **Virtual DOM Construction:** React builds a Virtual DOM, a representation of all React elements created from instances in the component tree.

- **Child Component Rendering:** When a component renders, all its child components are rendered too. This is necessary because React must determine if any children are affected by the parent's state changes.

- **Reconciliation with Fiber Tree:** The new Virtual DOM is reconciled with the existing _Fiber Tree_ (React's internal structure before the state update). This reconciliation process is carried out by React's reconciler, known as "Fiber".

- **Updated Fiber Tree:** The outcome of reconciliation is an updated Fiber Tree, which will be used for actual DOM updates.

#### Reconciliation

- **Efficiency in Updates:** React updates only the necessary parts of the DOM to reflect state changes, avoiding full DOM updates for performance reasons.

- **Reconciler Role:** The reconciler, currently Fiber, is key in React's architecture. It determines the minimal necessary changes to the DOM, making React efficient.

- **The result of the reconciliation process** is gonna be a list of DOM operations that are necessary to update the current DOM.

#### Fiber Reconciler

- **Fiber Reconciler:** During the initial render, Fiber takes the entire components tree (the virtual DOM), and based on it builds yet another tree which is the **Fiber Tree**.

- **Fiber Tree:** The fiber tree is a special internal tree where for each component instance and DOM element, there is one so-called **Fiber**.

- **Fibers:** Fibers are not recreated on every render (the fiber tree is never destroyed), instead it's a mutable data structure that is mutated over and over again in future reconciliation steps.

- **Fiber Role:** The state and props of any component instance are internally stored inside the corresponding fiber in the fiber tree. Each fiber also contains a queue of work to do, like updaitng state, registering side effects, DOM updates, etc.

- **Asynchronous Fiber Processing:** The linked list structure of the Fiber tree allows React to process updated asynchronously. This enables features like Suspense or transitions in React 18, allowing rendering to be paused and resumed without blocking the browser's JavaScript engine.

#### Reconciliation Process in Action

- **Outcode:** The reconciliation and diffing processes result is an updated Fiber tree and a list of necessary DOM updates. These are prepated for the next phase but not yet applied to the DOM.

### 3. Commit Phase

<br>
<br>
<br>

React writes to the DOM, updating inserting, deleting elements

The render phase resulted in a list of DOM updates, and this list will npw get used in the commit phase.

The commit phase is where React writes to the DOM. React goes through the effects list that was created during rendering, and applies them one by one to the actual DOM elements that were in the already existing DOM tree.

Committing is synchronoys (unlike the rendering phase, which can be paused): the DOM is updated in one go, it can't be interrupted. This is necessary so that the DOM never shows partial results, ensuring a consistent UI (in sync with state at all times). In fact, that's the whole point of dividing the entire process into the render phase and the commit phase, it's so that rendering can be paused, resumed and discarded, and then the result of all that rendering can be flushed to the DOM in one go.

After the commit phase completes, the `workInProgress` fiber tree becomes the `current` tree for the next render cycle, because fiber trees are never discarded or created from scratch, instead, they are reused in order to save precious rendering time.

The browser will then notice that the DOM has been changed and as a result, it will repaint the screen.

The render phase is performed by the React library. The React DOM library is responsible for the commit phase.

### 4. Browser Paint
