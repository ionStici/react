# Styles in React

## Styling Options For React Applications

Recommended tools for writing CSS in React apps: CSS Modules, Tailwind CSS, Styled Components.

| Styling Option    | Where?                          | How                   | Score       | Based On   |
| ----------------- | ------------------------------- | --------------------- | ----------- | ---------- |
| Inline CSS        | JSX elements                    | `style` prop          | JSX element | CSS        |
| CSS or Sass file  | External file                   | `className` prop      | Entire app  | CSS        |
| CSS Modules       | One external file per component | `className` prop      | Component   | CSS        |
| CSS-in-JS         | External file of component file | Creates new component | Component   | JavaScript |
| Utility-first CSS | JSX elements                    | `className` prop      | JSX element | CSS        |

**Alternative to styling with CSS:** UI libraries like MUI, Chakra UI, Mantine, etc.

## Inline Styles in React

In React, we insert CSS declarations inside an object, and asign that object to the `style` attribute of an JSX element.

```jsx
<h1 style={{ color: 'red' }}>Hello World</h1>
```

Object literals inside JSX using double curly braces:

1. The outer curly braces inject JavaScript into JSX.
2. The inner curly braces create a JavaScript object literal.

### General Rules

- In JSX, any hyphenated style properties are written using camelCase.
- Hyphenated words are invalid syntax for JS object properties.
- Style values must be declared as strings.
- Style values declared as numbers will be assumed as `px` units.
- After the source code is compiled, the styles will be rendered properly.

### Style Object

```js
export const styles = { fontSize: 25 };
<p style={styles}>Hello World</p>;
```

Store the styles inside an object and then inject that object into JSX.

We can reuse styles across multiple components by keeping the styles in a separate JavaScript file and then exporting and reusing them.

### Import a whole CSS file

```js
import './style.css';
```

This will make the CSS styles from `style.css` available to the importing module.
