# Framer Motion

[Framer Motion Official Website](https://www.framer.com/motion/)

```bash
npm install framer-motion
```

## The motion Component

The `motion` component will render exactly the HTML element we specify, but supercharged with performant animation capabilities.

```jsx
import { motion } from "framer-motion";

function App() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <>
      <motion.div
        animate={{ x: 10, y: 10, rotate: isOpen ? 180 : 0 }}
        transition={{ duration: 0.3, type: "spring" }}
      />

      <button onClick={() => setIsOpen(true)}>Rotate</button>
    </>
  );
}
```

- The `animate` prop is used to declaratively animate the element.
- The `transition` prop is used to configure the animation that will be played.
- We can conditionally animate the element using state as shown in the example.

## The initial and exit props

```jsx
import { motion, AnimatePresence } from "framer-motion";

function Component({ children, isOpen }) {
  return (
    <>
      <AnimatePresence>
        {isOpen && (
          <motion.dialog
            initial={{ opacity: 0, scale: 0.95 }}
            animate={{ opacity: 0, scale: 1 }}
            exit={{ opacity: 0 }}
          >
            {children}
          </motion.dialog>
        )}
      </AnimatePresence>
    </>
  );
}
```

1. Using the `initial` prop we can define the starting state of the animation.
2. The `animate` prop will animate the element immediately after it appears.
3. The `exit` prop is used to define the animation for when the element is removed from the DOM.

The `AnimatePresence` component is meant to be used as a wrapper around the code that conditionally displays or removes the animated elements. **We need this component for the `exit` animation to take effect.**
