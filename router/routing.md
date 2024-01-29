# What is Routing?

**Routing** is the process by which a web application uses the current browser URL (Uniform Resource Locator) to determine what content to show a user. By organizing an application’s content and displaying only what the user has requested to see, routing allows for rich, engaging, and clear user experiences.

**A Router** is tool that watches changes in the url and prevents de default browser behavior of sending a request in case the url updates, then it enables React to update the UI based on the new url string.

**The basic structure of URLs:**

- `https://` -> `codecademy.com` -> `/articles` -> `?search=node`
- (1) Protocol -> (2) Domain -> (3) Path -> (4) Query

Every URL is essentially a request for some resource and each component of the URL serves to specify which resource is desired. URLs consist of several components, some of which are mandatory and some of which are optional:

1. Which **protocol** is used to access a resource.
2. _The domain:_ specifies the website that hosts the resource. The domain serves as the entry point for your application.
3. _The path:_ identifies the specific resource or page to be loaded and displayed to the user. This is where routing begins!
4. _The optional query string:_ appears after a `‘?’` and assigns values to parameters. Common uses of query strings include search parameters and filters.

Depending on the kind of application you build, there are different ways to handle the requests coming into your server. A popular **back-end solution is to use the Express routing framework.**

**React Router is a popular frontend routing solution** designed specifically for React applications.

## Routing and Single-Page Applications (SPAs)

### Routing

- **Purpose of Routing:** With routing, we match **different URLs** to **different UI views** (react components): **Routes**
- **Navigation and UI Synchronization:** Routing enables users to navigate between different views of a web app. It keeps the UI in sync with the current browser URL.
- **Enabling SPAs:** A key advantage of routing is the ability to develop Single-Page Apps (SPAs). In these apps, different routes lead to various components without the need for page reloads.
- **React Router:** library for implementing routing in React. This library facilitates **client-side routing,** handling URL changes within the browser without server-side requests.

### Single-Page Applications

- **Client-Side Execution:** SPAs are executed entirely in the client's browser.
- **Routes and Components:** In SPAs, different URLs corresponds to different components. This mapping allows for a seamless user experience where different "pages" are actually different components rendered within the same webpage.
- **JavaScript-Driven:** Components are re-rendred by React in response to route changes.
- **No Page Reloads:** The page is never reloaded. Navigation between different parts of the application feels fluid and resembles the experience of using a native app.
- **Data Loading:** SPAs often fetch additional data as needed from web APIs. This data is then used to update the components dynamically.

_SPA Workflow on the Client:_

1. **Navigation Trigger:** The user interacts with a router link within the app.
2. **URL Change:** The URL in the browser changes to reflect the new router, but the page does not reload.
3. **DOM and Component Update:** React responds to this URL change by rendering the component associated with the new route. This often involves loading data from a web API and dynamically updating the DOM.
