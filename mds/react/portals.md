# React Portals

**React Portal** is a feature that allows an element to be rendered outside of the parent component's DOM structure, while still keeping the element in the original position of the component tree.

The `createPortal` method takes two arguments: the JSX to render and the DOM node to render it into.

```jsx
// Modal Window Component using createPortal
import { createPortal } from 'react-dom';

function Modal({ children, isOpen, onClose }) {
  if (!isOpen) return null;

  return createPortal(
    <div className="overlay">
      <div className="content">
        <button onClick={onClose}>Close</button>
        <div>{children}</div>
      </div>
    </div>,
    document.getElementById('modal-root')
  );
}
```

Portals is a great feature for elements that we want to stay on top of other elements, like modal windows and menus.
