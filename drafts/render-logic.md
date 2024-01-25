## Components, Instances, Elements

**Components:** are JS functions that return JSX.

**An Instance:** is a reference to a component. Each time a component is used, React creates a new instance of that component, which includes its own state and lifecycle.

**An Element:** (or JSX) is a built-in component (object) that describes a DOM node. Each JSX element is converted to a `React.createElement()` call.

**Note:** React calls the component instances internally when it renders.

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

<!-- When react's strict mode is activated in React 18, our effects will run twice, but only in development phase, that's why we usually get 2 console logs. -->

<br>
