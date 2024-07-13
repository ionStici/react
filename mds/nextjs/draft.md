# Server-Side Rendering (SSR)

## Table of Contents

- [Comparing CSR vs. SSR]()

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

## Types of SSR

1. **Static Site Generation (SSG):** HTML is generated at build time and served to all users.
2. **Dynamic SSR:** The HTML is generated on each request, making it ideal for pages with frequently changing data.

## CSR vs SSR: Data Fetching Timeline

### CSR

<br>
<br>
<br>

### CSR

In CSR, once the user requests a page, the server only sends an empty HTML document, a CSS file, and a big JavaScript bundle.

After the JS bundle has been downloaded and executed, the app notices that it needs to fetch some data. The app renders a quick spinner, and waits for the data to be fetched from an API endpoint. So here we have yet another client-server interaction.

After the data arrives, the app will re-render itself with that new data, after which we can say that we have our initial page load, or "Largest Content Paint".

### SSR

The server starts by fetching all the relevant data for the page, the server then takes the data, generates the page, and sends the whole finished product to the client.

When the page first reaches the client, it already has all the content that the user is interested in. This is when the "Content Paint" happens in this case. Much much earlier than in CSR.

### Take Away

1. In SSR, it's the server who initiates the data fetching.
2. The render part moves from the client to the server.
3. The Largest Content Paint happens much earlier than in CSR. That's why SSR is so useful in content-heavy sites, where users don't want for all the content to appear.

## Interactivity

In the SSR scenario, typically the client will still include a JavaScript bundle, which gets downloaded and executed just like before, and then a process called hydration happens.

Hydration is the process where static HTML becomes interactive by adding JavaScript to it.
