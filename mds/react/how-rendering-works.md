# How Rendering Works

## Table of Content

- [Components, Instances, Elements](#components-instances-elements)
- [Render Logic](#render-logic)
- [1. Render is Triggered](#1-render-is-triggered)
- [2. Render Phase](#2-render-phase)
- [3. Commit Phase](#3-commit-phase)
- [Summary](#summary)
- [How Diffing Works](#how-diffing-works)
- [The Key Prop](#the-key-prop)
- [The Virtual DOM](#the-virtual-dom)

## Components, Instances, Elements

**Components:** are JS functions that return JSX.

**An Instance:** is a reference to a component. Each time a component is used, React creates a new instance of that component, which includes its own state and lifecycle.

**An Element:** (or JSX) is a built-in component (object) that describes a DOM node. Each JSX element is converted to a `React.createElement()` call.

**Note:** React calls the component instances internally when it renders.

## Render Logic

- **Render Logic:** Code that lives at the top level of the component function.
- **Event Handler Functions:** Code executed as a consequence of an event.
- **Side Effects:** Modifications of any data outside the function scope.
- **Pure Functions:** Functions with **no** side effects.

### Rules for Render Logic

1. Components must be pure when it comes to render logic: given the same props (input), a component instance should always return the same JSX (output).

2. Render logic must produce no side effects: no interaction with the "outside world" is allowed. So, within the render logic:

   - Do NOT perform API calls
   - Do NOT start timers
   - Do NOT directly use the DOM API
   - Do NOT mutate objects or variables outside of the function scope
   - Do NOT update state (or refs): this will create an infinite loop

These side effects should be performed in event handler functions and using the `useEffect` hook.

Note: When react's strict mode is activated in React 18, our effects will run twice, but only in development phase, that's why we usually get 2 console logs.

## 1. Render is Triggered

_..by updating state somewhere_

Two situations that trigger renders: **(1) Initial render** of the application, **(2) State is updated** in a component instance **(re-render)**

Internally, the render process is triggered for the entire application, but the DOM is updated by React only where the updated happened.

## 2. Render Phase

The **Render Phase** in React involves determining how the DOM should be updated. This internal process within React does not produce visual changes immediately.

- **Component Rendering:** React traverses the entire component tree. It renders components that have triggered a re-render by calling their component functions. This generates updated React elements, forming a new Virtual DOM.

- **Virtual DOM Construction:** React builds a Virtual DOM, a representation of all React elements created from instances in the component tree.

- **Child Component Rendering:** When a component renders, all its child components are rendered too. This is necessary because React must determine if any children are affected by the parent's state changes.

- **Reconciliation with Fiber Tree:** The new Virtual DOM is reconciled with the existing _Fiber Tree_ (React's internal structure before the state update). This reconciliation process is carried out by React's reconciler, known as "Fiber".

- **Updated Fiber Tree:** The outcome of reconciliation is an updated Fiber Tree, which will be used for actual DOM updates.

### Reconciliation

- **Efficiency in Updates:** React updates only the necessary parts of the DOM to reflect state changes, avoiding full DOM updates for performance reasons.

- **Reconciler Role:** The reconciler, currently Fiber, is key in React's architecture. It determines the minimal necessary changes to the DOM, making React efficient.

- **The result of the reconciliation process** is gonna be a list of DOM operations that are necessary to update the current DOM.

### Fiber Reconciler

- **Fiber Reconciler:** During the initial render, Fiber takes the entire components tree (the virtual DOM), and based on it builds yet another tree which is the **Fiber Tree**.

- **Fiber Tree:** The fiber tree is a special internal tree where for each component instance and DOM element, there is one so-called **Fiber**.

- **Fibers:** Fibers are not recreated on every render (the fiber tree is never destroyed), instead it's a mutable data structure that is mutated over and over again in future reconciliation steps.

- **Fiber Role:** The state and props of any component instance are internally stored inside the corresponding fiber in the fiber tree. Each fiber also contains a queue of work to do, like updaitng state, registering side effects, DOM updates, etc.

- **Asynchronous Fiber Processing:** The linked list structure of the Fiber tree allows React to process updated asynchronously. This enables features like Suspense or transitions in React 18, allowing rendering to be paused and resumed without blocking the browser's JavaScript engine.

### Reconciliation Process in Action

- **Outcode:** The reconciliation and diffing processes result is an updated Fiber tree and a list of necessary DOM updates. These are prepated for the next phase but not yet applied to the DOM.

## 3. Commit Phase

- **DOM Updates:** The Render Phase results in a list of DOM updates. In the Commit Phase, React applies these updates to the actual DOM.

- **Synchronous Operation:** The Commit Phase is synchronous, the DOM is updated is one go, it can't be interrupted. This is necessary so that the DOM never shows partial results, ensuring a consistent UI. In fact, that's the whole point of dividing the entire process into the render and commit phase.

- **Transition to Current Tree:** Post Commit Phase, the `workInProgree` Fiber tree becomes the `current` tree for the next render cycle. This reuse of Fiber trees saves rendering time.

- **Division of Responsibilities:** The React library handles the Render Phase, while the React DOM library is responsible for the Commit Phase.

- **Browser Response:** Following DOM updates, the browser repaints the screen to reflect changes.

## Summary

The **Render Phase** is the first step in the process of updating the UI. During this phase, React does the following:

1. **Reconciliation:** React compares the new React elements (returned from your components' render functions) with the previous ones. This process is also known as "reconciliation." It uses the virtual DOM to perform this comparison efficiently.

2. **Virtual DOM Diffing:** React calculates the differences (or "diffs") between the old and new virtual DOM structures. It figures out what changes need to be made to the actual DOM to reflect the new state of the application.

3. **Purely Computational:** No changes are made to the actual DOM in this phase. This is why the Render Phase is considered "purely computational." This means React can pause, abort, or restart the work if needed.

4. **Performance Optimization:** This phase offers potential for performance optimization. Since no actual DOM changes are committed here, React can efficiently pause, abort, or restart the render as needed based on priority and system resources.

After the Render Phase comes the **Commit Phase**. This phase is about applying the changes to the actual DOM:

1. **DOM Updates:** React applies the changes it calculated during the Render Phase to the DOM. This could include updating text or attributes of DOM elements and inserting or deleting DOM elements.

2. **User-visible Changes:** Unlike the Render Phase, changes made during the Commit Phase are immediately visible to the user. Once the updates are applied to the DOM, the user interface reflects the new state of the application.

3. **Single Pass:** The Commit Phase happens in a single pass. React goes through the entire tree and makes all the DOM updates in one go.

4. **No Interruption:** Unlike the Render Phase, the operations in the Commit Phase cannot be interrupted. Once React starts making changes to the DOM, it completes the entire process.

## How Diffing Works

**Diffing** is part of the reconciliation process, it involves comparing elements between two renders, based on their position in the virtual DOM tree.

_The efficiency of diffing comes from two key assumptions:_

1. **Different Types, Different Trees:** If two elements differ in type (a `div` changing to a `header`), React assumes their trees will be different. This leads to the old tree being completely replaced by a new one.

2. **Stable Keys Indicate Stability:** Elements with a consistent and unique `key` prop are treated as stable across renders. This assumption significantly optimizes performance, reducing potential operations from O(n³) to O(n) for 1000 elements.

_In the diffing process, React considers two main scenarios:_

1. **Elements of Different Types at the Same Position:**

   - When an element's type changes at a given position in the tree (like a `div` to a `header`), React deems the entire element and its children invalid.
   - These elements are removed from the DOM, and a new element is built from scratch, including creating a new component instance if applicable.
   - This process applies to both DOM elements and component instances.

2. **The Same Element Type at the Same Position:**

   - If an element at a specific position remains the same between renders, it is kept in the DOM, along with its children and their state.
   - This preservation applies to both DOM elements and React components.
   - If there are any changes in props or attributes, React updates these specifics while maintaining the element and its type.

## The Key Prop

The `key` prop is a special prop that we can use in order to tell the diffing algorithm that an element is unique. This works for both DOM elements and React elements.

In practice this means that we can give each component instance a unique identification, which will allow React to distinguish between multiple instances of the same component type.

When a key stays the same across renders, the element will be kept in the DOM (even if the position in the tree changes). This is why we have to use keys in lists.

When a key changes between renders, the element will be destroyed and a new one will be created (even if the position in the tree is the same as before). Great to reset state, which is the second big use case of the `key` prop.

## Virtual DOM

### Understanding the DOM manipulation problem

Whenever the browser detects a change to the DOM, it rebuilds the _entire_ DOM only for that change, not considering that most elements were not updated. DOM manipulation is a slower process than most JavaScript operations.

- _The problem:_ multiple inefficient DOM updates affecting the website performance.
- _The solution:_ Virtual DOM.

### The Virtual DOM

By using a Virtual DOM, we prevent unnecessary DOM rebuilds, and only affect the elements that were updated, greatly improving the webpage performance and creating amazing user experiences.

- The Virtual DOM is a JavaScript object that represents a copy of the real DOM object.
- In React, for every DOM object, there is a corresponding Virtual DOM object.
- Manipulating the virtual DOM is faster because nothing gets drawn on screen.

Frameworks like Vue or React create their own representation of the DOM as a JavaScript object. Whenever a change is going to be made to the DOM, the framework makes a copy of this JavaScript object, makes the change to that copy, and compares the two JavaScript objects to detect what has changed and then it informs the browser of these changes and only those parts of the DOM are updated.

Making changes to JavaScript objects and comparing so that only specific parts of the real DOM is updated, is far faster than trying to do the same with common DOM manipulations.

### How it Works

- When a JSX element is rendered, every single virtual DOM object gets updated.
- Then, React compares the virtual DOM with a virtual DOM snapshot that was taken right before the update.
- React figures out exactly which virtual DOM objects have changed (process called "diffing").
- Then, React updates only the modified object on the real DOM.

React can update only the necessary parts of the DOM and improve the performance.
