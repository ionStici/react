# Framer Motion

[Framer Motion](https://www.framer.com/motion/) is a simple yet powerful motion library for React, allowing for easy and performant animations.

```bash
npm install framer-motion
```

## The `motion` Component

The `motion` component renders the specified HTML element with enhanced animation capabilities.

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

- The `animate` prop defines the animation for the element.

- The `transition` prop configures the animation.

- State can be used to conditionally animate elements.

When the values of `animate` change, the element will automatically transition to the new values.

## The `initial` and `exit` Props

The `initial` and `exit` props define the starting and ending states of an animation.

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

- `initial` : Defines the starting state of the animation.
- `animate` : Defines the state after the element appears.
- `exit` : Defines the animation when the element is removed.

The `AnimatePresence` component is needed for the `exit` animation to work.

## Hover, Focus, Active States

```jsx
return (
  <motion.button
    whileHover={{ scale: 1.1 }}
    whileTag={{ scale: 1 }}
    whileFocus={{ backgroundColor: "red" }}
    transition={{ type: "spring", stiffness: 500 }}
  >
    Click
  </motion.button>
);
```

- `whileHover` : Defines the animation when the element is hovered.
- `whileTap` : Defines the animation when the element is tapped.
- `whileFocus` : Defines the animation when the element is focused.

## Variants

Variants allow defining multiple animation states and reusing them across components.

```jsx
function Modal({ children }) {
  const variants = {
    hidden: { opacity: 0, y: 50, scale: 0.95 },
    visible: { opacity: 1, y: 0, scale: 1 },
  };

  return (
    <motion.dialog
      variants={variants}
      initial="hidden"
      animate="visible"
      exit="hidden"
    >
      {children}
    </motion.dialog>
  );
}
```

Variants can be used to coordinate animations between parent and child components.

```jsx
function ModalContent() {
  return (
    <Modal>
      <motion.div
        variants={{
          hidden: { opacity: 0, scale: 0.5 },
          visible: { opacity: 1, scale: 1 },
        }}
        transition={{ type: "spring" }}
      />
    </Modal>
  );
}
```

Child components can inherit variant animations from parent components.

## Staggering

Staggering animations allows for delaying the start of each animation in a sequence.

```jsx
return (
  <motion.ul variants={{ visible: { transition: { staggerChildren: 0.05 } } }}>
    {items.map((item) => (
      <li initial={{ opacity: 0 }} animate={{ opacity: 1 }}></li>
    ))}
  </motion.ul>
);
```

Staggering helps create more dynamic and engaging animations by offsetting the start times of animations within a group.

## Keyframes

```jsx
return <motion.li animate={{ scale: [0.8, 1.3, 1] }}></motion.li>;
```

## The useAnimate Hook

```jsx
import { motion, useAnimate, stagger } from "framer-motion";

function Component() {
  const [input, setInput] = useState("");
  const [scope, animate] = useAnimate();

  if (!input) {
    animate(
      "input, textarea",
      { x: [-10, 0, 10, 0] },
      { type: "spring", duration: 0.2, delay: stagger(0.05) }
    );
  }

  return (
    <form ref={scope}>
      <input />
    </form>
  );
}
```
