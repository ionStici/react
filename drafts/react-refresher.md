# React Refresher

[ReactJS](https://react.dev/) is a declarative, component-based, client-side JavaScript library used for building reactive user interfaces. Using React we can build Single-Page-Applications (SPA).

<br>

## Notes from "React Refresher"

The react code is processed in the build step, this means that the code we write will not be the code that ends up in the browser, we simply write code that is convenient for us the developer, then behind the scenes that code will be transformed before it reaches the browser.

- `index.js` file is the starting point of the react application. In here, all the react code is imported. After which, the `render` method will insert all these code in a single html document right inside the `div` element with the `root` id, but only after the build step.

- `App.js` contains a function component that returns JSX and which we export so that `index.js` could import and render that in the browser.

**JSX** is a special syntax, converted in the build step into code that the browser understands.

Read this: [className JS property](https://developer.mozilla.org/en-US/docs/Web/API/Element/className)

React components must return html elements (referred to as JSX in this context) that will eventually render on the screen, or `null` in case we don't want to render anything.

All the html elements that we use inside a react component, under the hood are built-in components as well, with additional functionalities such as `onClick` for event listening, and many more. But, the components that we create are not built-in, they are a different kind of component.

React components should start with a capital letter, so that we could differentiate them from other regular html elements.

In case a JSX element has no content, we can omit the closing tag by including a slash `<div />`

`const [modal, setModal] = React.useState(false)` The `useState` function returns an array with 2 elements. We use JS destructuring to store these 2 elements. The first element is the actual state value. The second element is a function that can update the state value.

`{state ? <Comp /> : null}` this is how we render content conditionally with React.

Trick: `{state && <Comp />}` if both conditions are true, then the second value is returned.

In JavaScript, functions are first-class objects, we can pass them around as values just as you can pass strings and arrays and objects and numbers as values.

`props` for building re-usable components.

`useState` for changing what we see on the screen dynamically.

"Wrapper" Components - `props.children` is a special which every component receives by default. And `children` always holds the content which is passed between the opening and closing component tags.

`useRef` - access DOM elements.

### Router

A Router is tool that watches changes in the url and prevents de default browser behavior of sending a request in case the url updates, then it enables React to update the UI based on the new url string.

<br>

## Forms

With React and SPAs, you typically need a backend API to which you send requests. So a backend which does not send back html, but which instead expects and returns data.

Because fo security reasons, we need a backend api server to which we can send requests and then it's that server which connects to a database and stores data.

Firebase (from google) is a service which contains a databse and an API to which we can consent requests, which will then ensure that the data is saved in that database.
