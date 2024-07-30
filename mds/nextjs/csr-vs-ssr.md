# Comparing CSR and SSR

## Client-Side Rendering (CSR)

- **Rendering Process:** The HTML is rendered in the browser by JavaScript.

- **Performance:** The initial page load may be slow due to larger JavaScript bundle and data fetching after the components are mounted.

- **SEO Impact:** SEO can be challenging as content is not immediately available to search engines.

- **When to Use:** Ideal for highly interactive web apps. Suitable for internal tools that are behind a login and don't need to be indexed by search engines.

## Server-Side Rendering (SSR)

- **Rendering Process:** The HTML is rendered on the server and then sent to the client.

- **Performance:** The initial page load is faster due to reduced JavaScript and server-side data fetching.

- **SEO Impact:** SEO-friendly as content is readily available for search engine indexing.

- **When to Use:** Essential for content-driven websites like e-commerce, blogs, news pages, and marketing websites where SEO is critical.

## Initial Request: CSR vs SSR

- **CSR:** The server sends to the client an empty HTML document, a CSS file, and a large JS bundle. On the client, JS builds the HTML, then fetches some data, in the meantime displays a spinner, and once the data arrives the app re-renders with new data, completing the initial page load.

- **SSR:** The server fetched all the necessary data, it generates the page using that data, and sends the complete HTML to the client (CSS and JS as well). JavaScript is then used to make the page interactive.

- **Takeaways:** In SSR, it's the server who initiates the data fetching. The rendering process moves from the client to the server. The First Content Paint occurs much earlier in SSR.

## Interactivity and Hydration

During SSR, once the browser rendered the static HTML, a process known as **hydration** will begin, where the JS bundle adds interactivity to the static HTML.

In this process, React reconstructs the component tree on the client side and compares it to the existing server-rendered DOM currently on the page. If the two match, React will adopt the existing DOM, attach event handlers, and initiate any necessary side effects. Instead of creating new DOM elements, which can be time consuming, React simply adopts the existing DOM during hydration, making the page interactive.
