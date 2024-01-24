# React Ecosystem

<br>

## Libraries vs. Frameworks & The React Ecosystem

**Framework** (all-in-one kit) - everything you need to build a complete application is included in the framework. Frameworks like Angular includes stuff like HTTP requests, styling, routing, form management, and more. The downside of this is that you're stuck with the framework's tools and conventions (which is not always bad).

**Library** -

React is a view library (not a framework). If you want to build a large-scale SPAs, you will need to include many 3rd-party external libraries for things like routing, styling, http requests, and so on, so all these functionalities are not part of React itself. You can choose multiple 3rd-party libraries to build a complete application. You need to research, download, learn, and stay-up-to-date with multiple external libraries.

React has a large ecosystem of libraries that we can include in our React projects for different needs.

- Routing: **React Router**, React Location
- HTTP Requests: **fetch()**, Axios
- Remote state management: **React Query**, SWR, Apollo
- Global state management: **Context API**, **Redux**, Zustand
- Styling: **CSS Modules, Styled Components, tailwindcss**
- Form management: **React Hook Form**, Formik
- Animations/transitions: Framer Motion, react-spring
- UI components: Chakra, mantine

Next.js, Remix, Gatsby are "opinionated" React frameworks built on top of React, they extend React's functionalities, providing solutions for routing, state management, styling, and more.

We can consider this React frameworks as full-stack frameworks because they provide features like server-side rendering (SSR) and static-side generation (SSG), all by using React as the baselayer.
