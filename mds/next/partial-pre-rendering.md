# Partial Pre-Rendering (PPR)

Not all pages need to be entirely static or fully dynamic. In a typical Next.js application, each route is classified as either static or dynamic. But what if we could combine these approaches?

Consider a website that is mostly static, except for a navigation bar that shows the currently logged-in user's name. Normally, this would require the entire page to be dynamically rendered, as the username is only known at request time. However, this would be inefficient, as most of the page content doesn't change per user. Partial Pre-Rendering addresses this issue by blending static and dynamic rendering.

**Partial Pre-Rendering (PPR):** A new rendering strategy that allows combining static and dynamic rendering within the same route.

## How it Works

1. A static (pre-rendered) shell of the page is served immediately from a CDN, with placeholders for dynamic content.
2. The dynamic content, which may take longer to render, is streamed in as it becomes available from the server.

This approach results in faster page loads, as most of the content can be delivered from the edge (CDN), even if there are some dynamic elements. This means that a page doesn't need to be entirely dynamically rendered just because a small part relies on request-time data or uncached responses.

- **Configuration:** [PPR needs to be enabled in the Next.js configuration file](https://nextjs.org/learn/dashboard-app/partial-prerendering#implementing-partial-prerendering).

- **Static Shell:** By default, as much content as possible is statically rendered, creating a static shell.

- **Dynamic Content:** Dynamic components should be placed within Suspense boundaries, which signal to Next.js that these areas contain dynamic content.

- **Boundary Function:** The boundary ensures that the dynamic part (like user-specific data or uncached API responses) does not affect the entire route.

- **Static Fallback:** A static fallback is provided while the dynamic parts are being rendered.

- **Insertion of Dynamic Components:** As dynamic data becomes available, these components or subtrees are inserted into the static shell.

This method optimizes performance by reducing the need for full page re-renders, leveraging both static and dynamic rendering as needed.
