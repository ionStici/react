[&larr; Back](./react-router.md)

## What is Routing?

**Routing** is the process by which a web application uses the current browser URL (Uniform Resource Locator) to determine what content to show a user. By organizing an application’s content and displaying only what the user has requested to see, routing allows for rich, engaging, and clear user experiences.

**The basic structure of URLs:**

- `https://` -> `codecademy.com` -> `/articles` -> `?search=node`
- Protocol -> Domain -> Path -> Query

Every URL is essentially a request for some resource and each component of the URL serves to specify which resource is desired. URLs consist of several components, some of which are mandatory and some of which are optional:

1. Which **protocol** is used to access a resource.
2. _The domain:_ specifies the website that hosts the resource. The domain serves as the entry point for your application.
3. _The path:_ identifies the specific resource or page to be loaded and displayed to the user. This is where routing begins!
4. _The optional query string:_ appears after a `‘?’` and assigns values to parameters. Common uses of query strings include search parameters and filters.

Depending on the kind of application you build, there are different ways to handle the requests coming into your server. A popular back-end solution is to use the Express routing framework.

_React Router_ is a popular frontend routing solution designed specifically for React applications.

<br>
