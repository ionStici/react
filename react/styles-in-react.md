# Styles in React

Recommended tools for writing CSS in React apps:

- CSS Modules
- Tailwind CSS
- Styled Components
- Styled JSX

## Inline Styles in React

In React, we insert CSS declarations inside an object, and asign that object to the `style` attribute of an JSX element.

```jsx
<h1 style={{ color: "red" }}>Hello World</h1>
```

Object literals inside JSX using double curly braces:

1. The outer curly braces inject JavaScript into JSX.
2. The inner curly braces create a JavaScript object literal.

## General Rules

- In JSX, any hyphenated style properties are written using camelCase.
- Hyphenated words are invalid syntax for JS object properties.
- Style values must be decalred as strings.
- Style values declared as numbers will be assumed as `px` units.
- After the source code is compiled, the styles will be rendered properly.

## Style Object

```js
export const styles = { fontSize: 25 };
<p style={styles}>Hello World</p>;
```

Store the styles inside an object and then inject that object into JSX.

## Share Styles Across Multiple Components

We can reuse styles across multiple components by keeping the styles in a separate JavaScript file and then exporting and reusing them.

## Import a whole CSS file

```js
import "./style.css";
```

This will make the CSS styles from `style.css` available to the importing module.

<br>
