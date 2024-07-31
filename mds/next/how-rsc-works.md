# How the RSC Architecture Works Behind the Scenes

## Table of Contents

- [How RSC works without SSR](#how-rsc-works-without-ssr)
- [The Relationship Between RSC and SSR](#the-relationship-between-rsc-and-ssr)
- [How RSC works together with SSR](#how-rsc-works-together-with-ssr)

## How RSC works without SSR

_A component tree containing both Server Components (SC) and Client Components (CC):_

1. All **server components** are rendered on the server, producing React elements. These elements contain only the output of the server components, so the code used to render these components is removed on the server. This elimination of code is the technical reason why state and hooks cannot be used in server components, as the resulting React elements must be serializable to be sent to the client, and functions are not serializable.

2. **Client components** are not rendered on the server. Instead, the component tree creates placeholders for where each client component will be rendered. These placeholders contain serialized props passed from server components and a URL to the script with the actual component code. This reference is necessary for rendering the client component on the client side. This complex process is managed by the framework's bundler, which uses RSC.

3. The result is a component tree with a mix of rendered server components and placeholders for client components, referred to as the **RSC Payload**. This RSC Payload is sent to the client in a JSON-like data structure. And then on the client, the client components are rendered as well, creating new React elements. And with this we have the complete virtual DOM on the client ready to be committed to the DOM in the usual way, finishing the rendering process.

## The Relationship Between RSC and SSR

RSC (React Server Components) and SSR (Server-Side Rendering) are distinct concepts, but they do complement each other.

**How Dynamic SSR Works in General** _(HTML generated at each incoming request)_: The server renders the component tree into HTML, which is then sent to the client. Along with the HTML, the JavaScript bundle is sent to the browser for hydration.

**RSC with SSR:** SSR complements RSC by generating the full HTML output for both server and client components, which speeds up the initial page load.

## How RSC works together with SSR

- Server Components are rendered entirely on the server.

- Client Components are rendered on the server to generate the initial HTML, providing a fully rendered page.

- The server-rendered HTML, including the static output from both server and client components, is sent to the client.

- This results in a fast initial page load, as the static content is displayed quickly.

- The rendering of server and client components operates within two distinct environments: the React server for server components and the React client for client components.

- Once the HTML reaches the client, React renders and hydrates the client components, enabling full interactivity on the page.

Client components are not rendered on the server in the traditional sense. Instead, they are still represented by placeholders in the server-rendered output, which include details like props and references to their JavaScript code. The actual rendering of these components, where they become fully functional and interactive, occurs during hydration on the client side. The purpose of initially processing client components on the server is to generate the necessary HTML structure for a quick initial page load, ensuring fast delivery of content to the client.

### Key Distinction

- **RSC without SSR:** Server components are rendered on the server. Client components are entirely rendered on the client side, with only placeholders provided by the server. This means the client does most of the work in rendering the client components.

- **RSC with SSR:** The server generates the full HTML output for both server and client components, which speeds up the initial page load and provides a better user experience with immediate content. However, the interactivity still requires client-side rendering and hydration.
