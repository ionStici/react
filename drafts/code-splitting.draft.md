# Draft

## Optimizing Bundle Size

- **Bundle:** (produced by a tool like vite) JavaScript file containing the **entire application code.** Downloading the bundle will load **the entire app at once**, turning it into a SPA

- **Bundle size:** Amount of JavaScript users have to download to start using the app. One of the most important things to be optimized, so that the bundle takes **less time to download**

- **Code splitting:** Splitting bundle into multiple parts that can be **downloaded over time** ("lazy loading")

<br>

## Code Splitting

```jsx
const Homepage = lazy(() => import("./pages/Homepage"));
```
