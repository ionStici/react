# Tailwind CSS

The best place to learn Tailwind CSS is from the [official documentation](https://tailwindcss.com/docs/installation).

## What is Tailwind CSS

[**Tailwind CSS**](https://tailwindcss.com/) is a utility-first CSS framework packed with classes that can be composed to build any design, directly in your markup.

**Utility-first CSS approach:** writing tiny classes with one single purpose, and then combining them to build entire layouts. In tailwind, these classes are already written for us. So we're not gonna write any new CSS, but instead use some of tailwind's hundreds of classes.

**Tailwind advantages:** You don't need to think about class names; No jumping between files to write markup and styles; Immediately understand styling in any project that uses tailwind; Tailwind is a design system: many design decisions have been taken for your, which makes UIs look better and more consistent; Saves a lot of time, e.g. on responsive design; Docs and VS Code integration are great;

## Setting Up Tailwind CSS

_Tailwind setup example with Vite and React._

[Installation](https://tailwindcss.com/docs/installation) -> Framework Guides -> Vite -> Using React.

```bash
npm install -D tailwindcss postcss autoprefixer
npx tailwindcss init -p
```

This will create the `postcss.config.js` and `tailwind.config.js` files. Add the following to the `tailwind.config.js` file:

```js
/** @type {import('tailwindcss').Config} */
export default {
  content: ["./index.html", "./src/**/*.{js,ts,jsx,tsx}"],
  theme: { extend: {} },
  plugins: [],
};
```

### Preflight

Add the following to the `./src/index.css` file:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

This will inject a [set of base styles](https://tailwindcss.com/docs/preflight) for your tailwind project.

### VS Code Extension

[Tailwind CSS IntelliSense](https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss)

### Prettier Plugin

[Prettier plugin for Tailwind CSS](https://github.com/tailwindlabs/prettier-plugin-tailwindcss)

```bash
npm install -D prettier prettier-plugin-tailwindcss
```

```json
// .prettierrc
{ "plugins": ["prettier-plugin-tailwindcss"] }
```

### Using Tailwind in JSX

```jsx
function App() {
  return <h1 className="text-center text-xl">Hello World</h1>;
}
```
