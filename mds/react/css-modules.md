# CSS Modules

<!-- CSS Modules are not a standalone technology that you install like a package or library; instead, they are typically used in conjunction with build tools and JavaScript frameworks or libraries. -->

[**CSS Modules**](https://github.com/css-modules/css-modules) are CSS files in which all class names and animation names are scoped locally by default.

CSS Modules is not an official implementation in the browser, but rather a process in the build step that changes class names and selectors to be scoped. Using CSS modules requires a module bundler like webpack.

**Why CSS Modules?** Modular and reusable CSS. No more conflicts. Explicit dependencies. No global scope.

**BEM is not required in CSS Modules.** Classes are scoped very deliberately, they won’t conflict. Specificity issues disappear with CSS Modules.

## Importing and Using CSS Modules in React

- We can use both CSS and Sass files (but first install Sass).
- We must specify that the file is a module: `[name].module.css`. This naming convention lets React and Webpack know that we are using CSS Modules.

```js
import styles from './style.module.css';
import colors from './colors.module.css';
import padding from './padding.module.scss';
const text = <p className={(styles.pText, colors.red, padding.sm)}>Hello</p>;
```

**Naming classes:** For local class names camelCase naming is recommended.

## Global Styles

```css
/* Using Usual CSS */
:global(.globalClassName) {
  color: red;
}

/* Sass */
:global {
  .global-class-name {
    color: green;
  }
}
```

- This will declares the stuff in parenthesis in the global scope.
- Similarly, `:local` and `:local(...)` for local scope.

## Composition: The composes keyword

```css
/* colors.css */
.red {
  color: red;
}
```

```css
/* style.css */
.className {
  composes: red from './colors.css';
}

.fontFamily {
  font-family: monospace;
}

.p {
  composes: className fontFamily;
  font-size: 16px;
}
```

```js
.text {
  composes: fontSize from global;
}
```

```js
import styles from "./style.css";
element.innerHTML = `<p class=`${styles.p}`>Hello</p>`;
```

- Classes are bound to the element by the use of the `composes` keyword.
- **Rule:** `composes` rules must be before other rules.
- We can compose multiple classes in the same declaration.
- We can even compose class names from other CSS Modules.
- We can compose from **global** class names.

<!-- ```jsx
<p className={styles['btn--active']}>Hi</p>
``` -->
