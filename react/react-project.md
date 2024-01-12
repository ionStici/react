# New React Project

## React using Vite

```
npm create vite@latest my-app
```

### Predefined Scripts

- `npm run dev` starts the development server.

- `npm run build` bundles the app into static files for production.

- `npm run preview` preview the production version of the app.

### Directory Structure

- `public/` for static assets.

- `src/` contains the actual application code.

- `package.json` contains descriptive and functional metadata about a project.

- `package-lock.json` contains the dependency tree installed in node_modules/.

- `node_modules/` contains dependencies and sub-dependencies used by React.

## Rendering a Component

_Boilerplate Code_

```jsx
// index.jsx
import React from "react";
import ReactDOM from "react-dom/client";

function App() {
  const text = "Hello, React!";
  return <h1>{text}</h1>;
}

const root = ReactDOM.createRoot(document.getElementById("root"));

root.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

- The `React` and `ReactDOM` libraries are imported.

- A functional component named `App` is defined.

- `ReactDOM.createRoot()` creates the root DOM node.

- The `render()` method is called on the `root` obhect, which mounts the `App` component to the DOM.

- `<React.StrictMode>` is an optional utility component that helps detect potential problems.

_Virtual DOM note:_ `render` only updates DOM elements that have been changed.

<br>
