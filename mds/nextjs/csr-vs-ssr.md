<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Comparing CSR vs. SSR

### Client-Side Rendering (CSR)

- **Rendering Process:** HTML is rendered in the browser by JavaScript.
- **Performance:** Slower initial page loads due to larger JavaScript bundle and data fetching after components mount.
- **SEO Impact:** SEO can be challenging as content is not immediately available to search engines.

### Server-Side Rendering (SSR)

- **Rendering Process:** HTML is rendered on the server. The server sends the fully generated webpage to the client.
- **Performance:** Faster initial page loads due to reduced JavaScript and server-side data fetching.
- **SEO Impact:** SEO-friendly as content is readily available for search engine indexing.

### When to Use CSR

- **Single Page Apps (SPAs):** Ideal for highly interactive web apps.
- **Non-SEO Critical Apps:** Suitable for internal tools or apps that are behind a login and don't need to be indexed by search engines.

### When to Use SSR

- **SEO-Driven Websites:** Essential for content-driven websites like e-commerce, blogs, news sites, and marketing websites where SEO is critical.

### Types of SSR

1. **Static Site Generation (SSG):** HTML is generated at build time and served to all users.
2. **Dynamic SSR:** The HTML is generated on each request, making it ideal for pages with frequently changing data.

## Initial Page Load and Data Fetching

### CSR

1. **Initial Request:** The server sends an empty HTML document, a CSS file, and a large JavaScript bundle.
2. **JavaScript Execution:** After downloading and executing the JS bundle, the app starts fetching data.
3. **Data Fetching:** The app displays a loading spinner while waiting for data from an API.
4. **Rendering:** Once the data arrives, the app re-renders with new data, completing the initial page load (Largest Content Paint).

### SSR

1. **Initial Request:** The server fetches all the necessary data for the page.
2. **Server-Side Rendering:** The server generates the page with the data and sends the complete HTML to the client.
3. **Initial Load:** The client receives the fully rendered page with all the content, leading to a much earlier "Content Paint" compared to CSR.

### Key Takeaways

1. **Data Fetching:** In SSR, it's the server who initiates the data fetching.
2. **Rendering:** The rendering process moves from the client to the server.
3. **Performance:** Largest Content Paint occurs much earlier in SSR, making it beneficial for content-heavy sites where quick content visibility is crucial.

### Interactivity and Hydration

Even in SSR, the client typically downloads and executes a JavaScript bundle. This leads to a process called **hydration**, where static HTML becomes interactive by adding JavaScript functionality.

In SSR, after the generated webpage is sent and rendered on the client so that the first content paint happens, the React bundle is downloaded as well to hydrate the static webpage. In this process, the React will build back the component tree on the client and will compare it to the actual server-side rendered DOM that's currently on the page, and if it match then react will simply adopt the existing DOM, and attach all event handlers and file off existing effects. So instead of creating brand new DOM elements, which can take a long time, in hydration, react simply attempts to adopt the already existing DOM. After this process finishes, the page becomes interactive.

**Common Hydration Errors:** incorrect HTML element nesting, rendering different data on the server and on the client, using browser-only APIs like window or localStorage, incorrect use of side effect, etc.
