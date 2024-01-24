# Draft

# Virtual DOM

## Understanding the DOM manipulation problem

Whenever the browser detects a change to the DOM, it rebuilds the _entire_ DOM only for that change, not considering that most elements were not updated. DOM manipulation is a slower process than most JavaScript operations.

- _The problem:_ multiple inefficient DOM updates affecting the website performance.
- _The solution:_ Virtual DOM.

## The Virtual DOM

By using a Virtual DOM, we prevent unnecessary DOM rebuilds, and only affect the elements that were updated, greatly improving the webpage performance and creating amazing user experiences.

- The Virtual DOM is a JavaScript object that represents a copy of the real DOM object.
- In React, for every DOM object, there is a corresponding Virtual DOM object.
- Manipulating the virtual DOM is faster because nothing gets drawn on screen.

Frameworks like Vue or React create their own representation of the DOM as a JavaScript object. Whenever a change is going to be made to the DOM, the framework makes a copy of this JavaScript object, makes the change to that copy, and compares the two JavaScript objects to detect what has changed and then it informs the browser of these changes and only those parts of the DOM are updated.

Making changes to JavaScript objects and comparing so that only specific parts of the real DOM is updated, is far faster than trying to do the same with common DOM manipulations.

## How it Works

- When a JSX element is rendered, every single virtual DOM object gets updated.
- Then, React compares the virtual DOM with a virtual DOM snapshot that was taken right before the update.
- React figures out exactly which virtual DOM objects have changed (process called "diffing").
- Then, React updates only the modified object on the real DOM.

React can update only the necessary parts of the DOM and improve the performance.

# React Developer Tools

[React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi) extension allows to inspect React components.

In Chrome DevTools, we get two new tabs: **Components** and **Profiler**.

<br>

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

2. **Render Phase** - react calls all component functions and figures out how DOM should be updated, this happens internally inside React and does not produce visual changes yet

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

The render phase resulted in a list of DOM updates, and this list will npw get used in the commit phase.

The commit phase is where React writes to the DOM. React goes through the effects list that was created during rendering, and applies them one by one to the actual DOM elements that were in the already existing DOM tree.

Committing is synchronoys (unlike the rendering phase, which can be paused): the DOM is updated in one go, it can't be interrupted. This is necessary so that the DOM never shows partial results, ensuring a consistent UI (in sync with state at all times). In fact, that's the whole point of dividing the entire process into the render phase and the commit phase, it's so that rendering can be paused, resumed and discarded, and then the result of all that rendering can be flushed to the DOM in one go.

After the commit phase completes, the `workInProgress` fiber tree becomes the `current` tree for the next render cycle, because fiber trees are never discarded or created from scratch, instead, they are reused in order to save precious rendering time.

The browser will then notice that the DOM has been changed and as a result, it will repaint the screen.

The render phase is performed by the React library. The React DOM library is responsible for the commit phase.

### 4. Browser Paint

<br>

## How Diffing Works

The diffing algorithm that is part of the reconciliation process.

Diffing is comparing elements step-by-step between 2 renders based on thei position in the tree.

Diffing uses 2 fundamental assumptions (rules): (1) Two elements of different types will produce different trees, (2) Elements with a stable key prop stay the same across renders. This allows React to go from 1.000.000.000 [O(n3)] to 1000 [O(n)] operaitons per 1000 elements.

Two different situations that need to be considered:

1. Having 2 different elements at the same position in the tree between 2 renders.

   Changing a DOM element simply means that the type of the element has changed, for example from a `div` to a `header`. In a situation like this, React will assume that the element itself plus all its children are no longer valid, therefore all these elements will be destroyed and removed from the DOM (including their state).

   After removing from the DOM, a new element will be rebuild with a brand new component instance as the child.

   All this works the same for component instances.

2. Having the same element at the same position in the tree, between 2 renders.

   If after a render a element at a certain position in the tree is the same as before, the element will simply be kept in the DOM, as well all child elements including their state.

   The same element at the same position in the tree stays the same and preserves state, and it works like this for DOM elements and for React elements as well.

   New props or attributes are passed if they changed between renders without changing the element type.

These are the only 2 situations that matter.

<br>

## The Key Prop

The Key Prop is a special prop that we can use to tell the diffing algorithm that an element is unique. This works for both DOM elements and React elements.

In practice this means that we can give each component instance a unique identification, which will allow React to distinguish between multiple instances of the same component type.

When a key stays the same across renders, the element will be kept in the DOM (even if the position in the tree changes). This is why we have to use keys in lists.

When a key changes between renders, the element will be destroyed and a new one will be created (even if the position in the tree is the same as before). Great to reset state, which is the second big use case of the key prop.

<br>

## Rules for Render Logic: Pure Components

**Render Logic** - Code that lives at the top level of the component function; Participates in describing how the component view looks like; Code executed every time the component renders;

**Event Handler Functions** - Code executed as a consequence of the event that the handler is listening for; Code that does things: update state, perform an HTTP request, read an input field, navigate to another page, etc.

**Side effects** - modification of any data outside the function score (mutating external variables, HTTP requests, writing to DOM);

**Pure functions** - a function that has **no** side effects; Functions that do not change any variable outside their scope; Given the same input, a pure function always returns the same output;

### Rules for Render Logic

1. Components must be pure when it comes to render logic: given the same props (input), a component instance should always return the same JSX (output).

2. Render logic must produce no side effects: no interaction with the "outside world" is allowed. So, in render logic:

   - Do NOT perform API calls
   - Do NOT start timers
   - Do NOT directly use the DOM API
   - Do NOT mutate objects or variables outside of the function scope
   - Do NOT update state (or refs): this will create an infinite loop

These side effects, are only forbidden inside render logic. We have other options for running side effects: Side effects are allowed (and encouraged) in event handler functions (which are not part of the render logic). These is also a special hook to register side effects `useEffect`.

<br>

## State Update Batching

Renders are not triggered immediately, but scheduled for when the JS engine has some "free time". There is also batching for multiple `setState` calls in event handlers.

```jsx
// Event handler function
const reset = () => {
  setCount(0);
  console.log(count); // 7
  setModal(false);
  setPlay(true);
};
```

Considering an event handle function with multiple state updates, these state updated will get batched into just **one** state update for the entire event handler.

Updating multiple pieces of state won't immediately cause a re-render for each update, instead all pieces of state inside the envet handler are updated in one go, and only then will React trigger one single _render + commit_.

What will the value of `count` be when logging it? At the point of logging the `count` state, this state will still hold the state before update. At this point we say that the state is stale, meaning that the state is no longer fresh and updated, because a state update will only be reflected in the state variable after the re-render. For this reason, we say that updating state in react is asynchronous.

Updated state variables are not immediately available after `setState` call, but only after the re-render.

This also applies when only one state variable is updated.

If we need to update state based on previous update, we use `setState` with a callback.

Starting with React 18, state batching is available in timeout functions, promises and DOM native event methods:

```jsx
setTimeout(reset, 1000);
fetchStuff().then(reset);
btn.addEventListener("click", reset);
```

p.s. If we attempt to update the state while the new state is equal to the current state, React will not update the state and not re-render the component instance.

p.s. In case we try to update the same state multiple times in a row by computing the new state from the current state, the next state updates actually will not use the updated state but instead the current state for each update. Because the state remains stale.

<br>

## How Events Work in React

Event Delegation works in React as well.

When we register an event handler on a JSX element, for example using the `onClick` prop, behind the scenes React actually registers only one event handler per event type at the root node of the fiber tree during the render phase.

If we have multiple `onClick` handlers in our code, React will somehow bundle them all together and just add one big `onClick` handler to the `#root` node of the fiber tree.

Behind the scenes, React performs event delegation for all events in our application.

React delegates all events to the `#root` DOM container, where they will get handled, not in the place where we attach the `onClick` prop.

### Synthetic Events

When we declare and event handler like this one:

```jsx
<input onChange={(e) => setText(e.target.value)} />
```

..react gives us access to the event object that was created, just like in vanilla JS. However, in React, this event object is different.

In vanilla JS we get access to the native DOM event object, React on the other hand will give us something called a synthetic event, which is a thin wrapper around the DOM's native event object.

By wrapper, we mean that synthetic events are similar to native event objects, but they add or change some functionalities on top of them.

These synthetic events have the same interface as native event objects, including important methods like `preventDefault`. What's special about synthetic events thought, and one of the reasons why the React team decided to implement them is the fact that they fix some browser incosistencies, so that events work in the exact same way in all browsers. Most synthetic events actually bubble (including focus, blur, and change which usually do not bubble), except for scroll.

<br>

## Libraries vs. Frameworks & The React Ecosystem

**Framework** (all-in-one kit) - everything you need to build a complete application is included in the framework. Frameworks like Angular includes stuff like HTTP requests, styling, routing, form management, and more. The downside of this is that you're stuck with the framework's tools and conventions (which is not always bad).

**Library** -

React is a view library (not a framework). If you want to build a large-scale SPAs, you will need to include many 3rd-party external libraries for things like routing, styling, http requests, and so on, so all these functionalities are not part of React itself. You can choose multiple 3rd-party libraries to build a complete application. You need to research, download, learn, and stay-up-to-date with multiple external libraries.

React has a large ecosystem of libraries that we can include in our React projects for different needs.

- Routing: **React Router**, React Location
- HTTP Requests: **fetch()**, Axios
- Remote state management: **React Query**, SWR, Apollo
- Global state management: **Context API**, **Redux**, Zustand
- Styling: **CSS Modules, Styled Components, tailwindcss**
- Form management: **React Hook Form**, Formik
- Animations/transitions: Framer Motion, react-spring
- UI components: Chakra, mantine

Next.js, Remix, Gatsby are "opinionated" React frameworks built on top of React, they extend React's functionalities, providing solutions for routing, state management, styling, and more.

We can consider this React frameworks as full-stack frameworks because they provide features like server-side rendering (SSR) and static-side generation (SSG), all by using React as the baselayer.
