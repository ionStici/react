# New React Project

## React using Vite

```
npm create vite@latest my-react-app
```

### Predefined Scripts

- `npm run dev` starts the development server.

- `npm run build` bundles the app into static files for production.

- `npm run preview` preview the production version of the app.

## Rendering a Component

_Boilerplate Code_

```jsx
// main.jsx
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

- The `render()` method is called on the `root` object, which mounts the `App` component to the DOM.

- `<React.StrictMode>` is an optional utility component that helps detect potential problems.

_Virtual DOM note:_ `render` only updates DOM elements that have been changed.

### Directory Structure

- `public/` for static assets.

- `src/` contains the actual application code.

- `src/App.jsx` contains a function component that returns JSX and which is exported so that `src/main.jsx` could import and render that in the browser.

- `index.html` is the HTML document into which react will insert all the content, resulting in a SPA.

- `package.json` contains descriptive and functional metadata about a project.

- `package-lock.json` contains the dependency tree installed in node_modules/.

- `node_modules/` contains dependencies and sub-dependencies used by React.