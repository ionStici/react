# Partial Pre-Rendering (PPR)

Most pages don't need to be 100% static or 100% dynamic. Remember, each route in a next.js app is either static or dynamic. But what if it doesn't have to be that way?

For example, imagine a website that is fully static except for the navigation that displays the name of the currently logged in user. So this website will be 100% dynamically rendered, because that username can only be known at request time, but that's a huge waste. This is a problem that partial pre-rendering solves.

Partial Pre-Rendering: new rendering strategy that combines static and dynamic rendering in the same route.

_How it works:_

1. A static (pre-rendered) shell is served immediately from a CDN, leaving holes for dynamic content.
2. The slower dynamic content is streamed in as it's rendered on the server.

As a result, we end up with faster pages, that can mostly be delivered from the edge, even when there are small dynamically rendered parts on the page. This means that pages are no longer forced to be completely dynamically rendered just because one tiny part of the page depends on the incoming request or an uncached data request.

- PPR needs to be turned on in config file.
- By default, as much as possible of any route will be statically rendered, creating a static shell.
- Dynamic parts (components) should be placed inside Suspense boundaries
- These boundaries tell NExt.js that anything within the boundary is dynamic
- The boundary prevents the dynamic part (e.g. reading a header or making a non-cached fetch request) from spreading onto the entire route
- We provide a static fallback to be shown while the dynamic part is rendering
- Dynamic components or sub trees are inserted into the static shell as they become available
