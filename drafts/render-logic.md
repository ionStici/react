# Render Logic

- **Render Logic:** Code that lives at the top level of the component function.
- **Event Handler Functions:** Code executed as a consequence of an event.
- **Side Effects:** Modification of any data outside the function scope.
- **Pure Functions:** Functions with **no** side effects.

### Rules for Render Logic

1. Components must be pure when it comes to render logic: given the same props (input), a component instance should always return the same JSX (output).

2. Render logic must produce no side effects: no interaction with the "outside world" is allowed. So, within render logic:

   - Do NOT perform API calls
   - Do NOT start timers
   - Do NOT directly use the DOM API
   - Do NOT mutate objects or variables outside of the function scope
   - Do NOT update state (or refs): this will create an infinite loop

These side effects are only forbidden inside render logic. We have other options for running side effects: Side effects are allowed (and encouraged) in event handler functions (which are not part of the render logic). These is also a special hook to register side effects `useEffect`.

Note: When react's strict mode is activated in React 18, our effects will run twice, but only in development phase, that's why we usually get 2 console logs.

<br>
