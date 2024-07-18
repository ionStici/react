# Framer Motion

[Framer Motion](https://www.framer.com/motion/) is a simple yet powerful motion library for React, allowing for easy and performant animations.

```bash
npm install framer-motion
```

## Table of Contents

- [The `motion` Component](#the-motion-component)
- [The `initial` and `exit` Props](#the-initial-and-exit-props)
- [Hover, Focus, Active States](#hover-focus-active-states)
- [Variants](#variants)
- [Staggering](#staggering)
- [Keyframes](#keyframes)
- [The useAnimate Hook](#the-useanimate-hook)
- [The `layout` Prop](#the-layout-prop)
- [Orchestrating Multi-Element Animations](#orchestrating-multi-element-animations)
- [Animating Shared Elements](#animating-shared-elements)
- [Scroll-based Animations](#scroll-based-animations)

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

**Variants** are objects that contain named animation states.

You apply variants to a `motion` component by using the `variants` prop and specifying which variant should be active using the `initial`, `animate`, and `exit` props.

```jsx
function List({ items }) {
  const listVariants = { hidden: { opacity: 0 }, visible: { opacity: 1 } };
  const itemVariants = { hidden: { opacity: 0 }, visible: { opacity: 1 } };

  return (
    <motion.ul variants={listVariants} initial="hidden" animate="visible">
      {items.map((item) => {
        return (
          <motion.li variants={itemVariants} key={item}>
            {item}
          </motion.li>
        );
      })}
    </motion.ul>
  );
}
```

Variants can be used to coordinate animations between parent and child components. The parent component can define the overall animation states, and the child components can inherit and synchronize with those states.

From our code example, the `motion.li` components will implicitly use the `hidden` and `visible` states from `itemVariants` because the parent `motion.ul` component has `initial="hidden"` and `animate="visible"` props set. This means that the children (`motion.li`) will automatically inherit these states unless explicitly overridden.

## Staggering

Staggering animations allows for delaying the start of each animation in a sequence.

```jsx
return (
  <motion.ul transition={{ staggerChildren: 0.05 }}>
    {items.map((item) => (
      <li initial={{ opacity: 0 }} animate={{ opacity: 1 }}></li>
    ))}
  </motion.ul>
);
```

Staggering helps create more dynamic and engaging animations by offsetting the start times of animations within a group.

## Keyframes

Keyframes allow for defining animations that move through multiple states.

```jsx
return <motion.li animate={{ scale: [0.8, 1.3, 1] }} />;
```

## The useAnimate Hook

The `useAnimate` hook allows for programmatic control of animations.

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

The `useAnimate` hook provides greater flexibility and control over animations, allowing you to trigger animations based on state changes or other events.

## The `layout` Prop

To enable Framer Motion's layout animations, we simply set the `layout` prop of a `motion` component.

```jsx
return <motion.li layout />;
```

Any layout change that happens as the result of a re-render will be animated.

## Orchestrating Multi-Element Animations

To orchestrate multi-element animations, use the `AnimatePresence` component with the `mode` prop set to `"wait"`.

This approach ensures that animations of multiple elements are coordinated and performed sequentially.

```jsx
function Text({ text }) {
  return (
    <AnimatePresence mode="wait">
      <motion.p key={text} animate={{ scale: [1, 1.2, 1] }}>
        {text}
      </motion.p>
    </AnimatePresence>
  );
}
```

**Re-triggering Animations via Keys:** Changing the key of a component forces React to treat it as a new component, thereby re-triggering the animation.

## Animating Shared Elements

```jsx
return <motion.div layoutId="tag-indicator" />;
```

Using the `layoutId` prop allows elements with the same `layoutId` to smoothly transition between different layouts.

## Scroll-based Animations

- `useScroll` : A hook that provides the current scroll position.
- `useTransform` : A hook that maps input ranges (e.g., scroll positions) to output ranges (e.g., positions, scales, opacities).

```jsx
import { motion, useScroll, useTransform } from "framer-motion";

function Component({ children }) {
  // Destructure the vertical scroll position
  const { scrollY } = useScroll();

  // Transform the vertical position of the div based on scroll position
  const yDiv = useTransform(scrollY, [0, 200], [0, -100]);

  // Transform the opacity of the div based on scroll position
  const opacityDiv = useTransform(scrollY, [0, 200, 500], [1, 0.5, 0]);

  // Transform the vertical position of the text based on scroll position
  const yText = useTransform(scrollY, [0, 200, 300, 500], [0, 50, 50, 300]);

  // Transform the scale of the text based on scroll position
  const scaleText = useTransform(scrollY, [0, 300], [1, 1.5]);

  return (
    <motion.div style={{ opacity: opacityDiv, y: yDiv }}>
      <motion.h1 style={{ scale: scaleText, y: yText }}>Hello</motion.h1>
    </motion.div>
  );
}
```

`const yDiv = useTransform(scrollY, [0, 200], [0, -100]);` : Maps the `scrollY` value from `[0, 200]` to `[0, -100]`, which means as you scroll from 0 to 200 pixels, the `y` position of the `div` will animate from `0` to `-100` pixels.
