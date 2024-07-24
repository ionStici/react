# Comparing CSR and SSR

## Client-Side Rendering

- **Rendering Process:** HTML is rendered in the browser by JavaScript.
- **Performance:** Slower initial page loads due to larger JavaScript bundle and data fetching after components mount.
- **SEO Impact:** SEO can be challenging as content is not immediately available to search engines.

## Server-Side Rendering (SSR)

- **Rendering Process:** The server renders and sends the HTML to the client.
- **Performance:** Faster initial page loads due to reduced JavaScript and server-side data fetching.
- **SEO Impact:** SEO-friendly as content is readily available for search engine indexing.

## When to Use CSR

- **Single Page Apps (SPAs):** Ideal for highly interactive web apps.
- **Non-SEO Critical Apps:** Suitable for internal tools or apps that are behind a login and don't need to be indexed by search engines.

## When to Use SSR

- **SEO-Driven Websites:** Essential for content-driven websites like e-commerce, blogs, news sites, and marketing websites where SEO is critical.

## Types of SSR

1. **Static Site Generation (SSG):** The HTML is generated at build time by the bundler, and is served like that to all users.
2. **Dynamic SSR:** The HTML is generated on each request, making it ideal for pages with frequently changing data.

## Initial Page Load and Data Fetching

### Client-Side Rendering

1. **Initial Request:** The server sends an empty HTML document, a CSS file, and a large JavaScript bundle.
2. **JavaScript Execution:** After downloading and executing the JS bundle, the app starts fetching data.
3. **Data Fetching:** The app displays a loading spinner while waiting for data from an API.
4. **Rendering:** Once the data arrives, the app re-renders with new data, completing the initial page load (Largest Content Paint).

### Server-Side Rendering

1. **Initial Request:** The server fetches all the necessary data for the page.
2. **Server-Side Rendering:** The server generates the page with the data and sends the complete HTML to the client.
3. **Initial Load:** The client receives the fully rendered page with all the content, leading to a much earlier "Content Paint" compared to CSR.

### Key Takeaways

1. **Data Fetching:** In SSR, it's the server who initiates the data fetching.
2. **Rendering:** The rendering process moves from the client to the server.
3. **Performance:** Largest Content Paint occurs much earlier in SSR, making it beneficial for content-heavy sites where quick content visibility is crucial.

## Interactivity and Hydration

In Server-Side Rendering (SSR), the client typically downloads and executes a JavaScript bundle after receiving the initial static HTML. This process is known as **hydration**, where the static HTML is made interactive by adding JavaScript functionality.

During SSR, once the server-generated webpage is sent and rendered on the client, the JavaScript bundle hydrates the static webpage. In this process, React reconstructs the component tree on the client side and compares it to the existing server-rendered DOM currently on the page. If the two match, React will adopt the existing DOM, attach event handlers, and initiate any necessary effects. Instead of creating new DOM elements—which can be time-consuming, React simply adopts the existing DOM during hydration, making the page interactive.

**Common Hydration Errors:** Issues can arise from incorrect HTML element nesting, discrepancies between server-rendered and client-rendered data, using browser-specific APIs like `window` or `localStorage` during SSR, and improper handling of side effects. These can cause hydration mismatches or errors, leading to unexpected behavior or rendering issues.
