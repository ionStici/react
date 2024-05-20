# NextJS Essentials (App Router)

## Filesystem-based Routing

NextJS uses files & folders to define routes. Only files & folders inside the "app" folder are considered.

`page.js` file inside `/app` is a reserved filename (just like `layout.js`), `page.js` tells nextjs that it should render a page, this file contains a React server component.

NextJS works with react server components: Components which require a special "environment". NextJS provides such an environment. React Server Components are rendered only on the server, never on the client. It looks like a regular component, but nextjs ensures that this component is actually rendered on the server. If you add a console.log in that function, you will not see it in the browser console, istead you will see it on the backend (in the terminal / the process in the terminal is running the server). So it's a regular react component, but treated in a special way by nextjs, it is treated as a server component and executed on the server, **and it's then the returned JSX code that's sent over the wire to the browser to be rendered as HTML.**

## Adding Another Route via the File System

To add other route, add a new folder inside `app` folder, with a `page.js` file inside, the folder name will coincide with the router path.

Other reserved filenames:

`page.js` -> Define page content

`layout.js` -> Define wrapper around pages

`not-found.js` -> Define "Not Found" fallback page

`error.js` -> Define "Error" fallbcak page

The JSX code that the component function from `page.js` will be rendred as page content.

## Navigating Between Pages
