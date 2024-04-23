# CSS Modules

[**CSS Modules**](https://github.com/css-modules/css-modules) are essentially CSS files where all class names and animation names are scoped locally by default. This means that the styles defined in one module are not visible to any other modules unless explicitly imported.

CSS Modules aren't natively supported by web browsers. Instead, they function as part of your build process. During this phase, tools like webpack transform and scope class names and selectors to prevent styles from leaking across components.

**Key Benefits of CSS Modules:** Modularity and Reusability; No More Style Conflicts; Clean Global Scope; Simplified Specificity; Explicit Dependencies;

## Importing and Using CSS Modules in React

- We can use both CSS and Scss files with CSS Modules (but first install Sass).
- We must specify that a file is a module: `[name].module.css`.

  This naming convention lets React and Webpack know that we are using CSS Modules.

```jsx
import css from './styles.module.css';
import colors from './colors.module.css';

<p className={css.smText}>Hello</p>; // Using a class name
<p className={`${css.bigText} ${colors.red}`}>World</p>; // Multiple class names
```

We should use camelCase for local class name in react, otherwise we can:

```jsx
// Class names that contain dashes
<p className={css['btn--active']}>Hi</p>
```
