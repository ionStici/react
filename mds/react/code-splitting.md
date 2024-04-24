# Code Splitting

**Code Splitting** is a technique where you divide your code into bundles that can be loaded on demand, improving performance by reducing the size of the initial load. This means users only download the code necessary for the current view, improving the load time and efficiency of the application.

## Optimizing Bundle Size

- **Bundle:** (produced by a tool like vite) JavaScript file containing the **entire application code.** Downloading the bundle will load **the entire app at once**, turning it into a SPA.

- **Bundle size:** Amount of JavaScript that users have to download to start using the app. One of the most important things to be optimized, so that the bundle takes **less time to download**.

- **Code splitting:** Splitting bundle into multiple parts that can be **downloaded over time** ("lazy loading")

## Code Splitting in React

```jsx
import { Suspense, lazy } from 'react';
import Spinner from './components/Spinner';

const LazyComponent = lazy(() => import('./LazyComponent'));

function App() {
  return (
    <Suspense fallback={<Spinner />}>
      <LazyComponent />
    </Suspense>
  );
}
```

- **Dynamic `import()` Syntax :** import a module dynamically as a promise. When the module bundler encounters this syntax, it automatically starts code splitting your app.

- `React.lazy` is a function that allows you to render a dynamic import as a regular component. It is used to defer loading the component until it is required.

- `Suspense` is a component that lets you specify a loading indicator in case the component takes some time to load. It wraps lazy components and provides a fallback UI (like a spinner or a loading message).

### Practical Considerations

1. **Route-based Splitting:** This is the most common and recommended approach to split your code. You split each route's components into separate chunks, so they are only loaded when the route is visited.
2. **Error Handling:** When using `lazy`, ensure you handle errors with error boundaries, as network issues might prevent chunks from loading.
3. **Bundling:** Analyze your bundles using tools like `vite-bundle-visualizer` to identify and split large chunks effectively.
