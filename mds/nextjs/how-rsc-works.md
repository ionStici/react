# How the RSC Architecture Works Behind the Scenes

_A component tree containing both Server Components (SC) and Client Components (CC):_

1. All server components are rendered on the server, producing React elements. These elements contain only the output of the server components, so the code used to render these components is removed on the server. This elimination of code is the technical reason why state and hooks cannot be used in server components, as the resulting React elements must be serializable to be sent to the client, and functions are not serializable.

2. Client components are not rendered on the server. Instead, the component tree creates placeholders for where each client component will be rendered. These placeholders contain serialized props passed from a server component and a URL to the script with the actual component code. This reference is necessary for rendering the client component on the client side. This complex process is managed by the framework's bundler, which uses RSC.

3. The result is a component tree with both rendered and un-rendered component instances, known as the RSC Payload. This RSC Payload is sent to the client in a JSON-like data structure. And then on the client, the client components are rendered as well, also resulting in new React elements. And with this we have the complete virtual DOM on the client ready to be committed to the DOM in the usual way.

## The Relationship Between RSC and SSR

RSC (React Server Components) and SSR (Server-Side Rendering) are distinct concepts, but they do interact with each other.

### How SSR Works

**Dynamic SSR** _(HTML generated at each incoming request)_: The server renders the component tree into HTML, which is then sent to the client. Along with the HTML, the JavaScript bundle is sent to the browser for hydration.

### How SSR Works with RSC

When a framework combines RSC with SSR, both server components and client components are initially rendered on the server provided by Next.js. These components operate within two distinct environments: the React server for server components and the React client for client components. After the components have been rendered, the server sends the generated HTML to the browser (the JS bundle as well).

On the client side, client components are hydrated and fully rendered using the provided JavaScript bundle. This means that while the initial render for both types of components happens on the server, the actual execution and interactive behavior of client components occur in the browser. The RSC payload produced by server components is sent as well, so that react has access to the entire component tree.

Now, we have a complete react app in the browser that follows the RSC architecture. Note that SSR is only relevant on the initial render.
