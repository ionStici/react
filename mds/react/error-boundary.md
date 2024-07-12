# Error Boundary

To implement Error Boundary in functional components, we have to install a 3rd party library for it.

```
npm install react-error-boundary
```

- Wrap the application with the `ErrorBoundary` component.
- Provide a fallback component to the `FallbackComponent` prop that will be rendered if an error will occurs.
- The `onReset` is a function that is called when the error boundary's state is reset. Here you can execute additional logic to reset your application's state or handle cleanup operations necessary to attempt a recovery from the error.

```jsx
import { ErrorBoundary } from "react-error-boundary";
import Fallback from "components";

function App() {
  return (
    <ErrorBoundary
      FallbackComponent={Fallback}
      onReset={() => window.location.replace("/")}
    >
      <Home />
    </ErrorBoundary>
  );
}
```

The fallback component receives the `error` object and the `resetErrorBoundary` function that resets the error boundary and retries the render when called.

```jsx
function Fallback({ error, resetErrorBoundary }) {
  // Call resetErrorBoundary() to reset the error boundary and retry the render.

  return (
    <div>
      <p>Something went wrong: {error.message}</p>
      <button onClick={resetErrorBoundary}>Try again</button>
    </div>
  );
}
```
