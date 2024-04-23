# New React Project with Vite

```
npm create vite@latest my-react-app
```

## Predefined Scripts

- `dev` starts the development server

- `build` bundles the app into static files for production

- `preview` preview the production version of the app

## Rendering a Component

_Boilerplate Code_

```jsx
// main.jsx
import React from 'react';
import ReactDOM from 'react-dom/client';

function App() {
  return <h1>Hello, React!</h1>;
}

const root = ReactDOM.createRoot(document.getElementById('root'));

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

## Directory Structure

- `public/` folder for static assets.

- `src/` folder for the actual application code.

- `src/App.jsx` contains a function component that returns JSX and which is exported so that `src/main.jsx` could import and render it.

- `index.html` is the HTML document into which react will insert all the content.

- `package.json` contains descriptive and functional metadata about a project.

- `package-lock.json` contains the dependency tree installed in node_modules/.

- `node_modules/` contains dependencies and sub-dependencies used by React.
