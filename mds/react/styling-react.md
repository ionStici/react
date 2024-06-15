# Styling React Apps

## Table of Contents

- [Vanilla CSS](#vanilla-css)
- [Inline CSS in React](#inline-css-in-react)
- [CSS Modules](#css-modules)
- [Styled Components](./styled-comp.md)

## Vanilla CSS

```js
import './style.css';
```

Import a CSS file to make the styles available to the application.

## Inline CSS in React

In React, we can assign an object containing CSS declarations to the `style` attribute of an JSX element.

```jsx
<h1 style={{ color: 'red' }}>Hello World</h1>
```

- Hyphenated CSS properties use camelCase.
- CSS values must be declared as strings.

## CSS Modules

[**CSS Modules**](https://github.com/css-modules/css-modules) are essentially CSS files where all class names and animation names are scoped locally by default. This means that the styles defined in one module are not visible to any other modules unless explicitly imported.

CSS Modules aren't natively supported by web browsers. Instead, they function as part of your build process. During this phase, tools like webpack transform and scope class names and selectors to prevent styles from leaking across components.

**Key Benefits of CSS Modules:** Modularity and Reusability; No More Style Conflicts; Clean Global Scope; Simplified Specificity; Explicit Dependencies;

### Importing and Using CSS Modules in React

- We can use both CSS and Scss files with CSS Modules (but first install Sass).
- We must specify that a file is a module: `[name].module.css`.

  This naming convention lets React and Webpack know that we are using CSS Modules.

```jsx
import css from './styles.module.css';
import colors from './colors.module.css';

<p className={css.smText}>Hello</p>; // Using a class name
<p className={`${css.bigText} ${colors.red}`}>World</p>; // Multiple class names
<p className={css['btn--active']}>Hi</p>; // Class name that contains dashes
```
