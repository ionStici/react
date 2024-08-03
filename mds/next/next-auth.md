# NextAuth

NextAuth is a library that simplifies authentication in Next.js applications. This library is gonna make it easy to connect to many different auth providers. And we can also use our own users from our own database.

```
npm install next-auth@beta
```

**Authentication:** getting the right information about the current user and making sure that the user is who they claim to be.

**Authorization:** allow access to certain areas of the application to users that are logged in and have the right privilege to visit that part.

## Middleware in Next.js

Next.js Middleware: a function that executes between a incoming request and the response.

By default, middleware runs before every single route in our Next.js app, but we can specify which paths using a `matcher`. So, middleware runs after the request, but before the route the user is visiting is rendered and sent back.

Only one middleware function: needs to be exported from `middleware.js` in the project root folder.

The main function middleware is: reading incoming cookies and headers, setting cookies and headers on the response, this enables us to implement features such as authentication and authorization, server-side analytics, redirects based on geolocation, and many more.

The middleware functions always needs to produce a response, which can happen in two way, first and most common way to produce a response is that the middleware function redirects to some route in the app, the second way to produce a response is to send a JSON response to the client.

### Protecting Routes With NextAuth Middleware

`middleware.js` needs to be in the root of the directory, not in the `app` folder.

```js
// middleware.js

import { NextResponse } from "next/server";

export function middleware(request) {
  // console.log(request);
  return NextResponse.redirect(new URL("/about", request.url));
}

export const config = {
  matcher: ["/account", "/contact"], // specify the routes at which the middleware will execute
};
```

By default Middleware executes on every route redirect, so this can lead to an infinite loop. And so to make it stop and have to make it run for certain routes only using `matcher`.

#### Middleware with NextAuth

```js
// middleware.js

import { auth } from "./app/_lib/auth";

export const middleware = auth;

export const config = {
  matcher: ["/account"],
};
```

When accessing the `/account` route, the `authConfig.callbacks.authorized` function will be called.

Server Actions: add interactivity to server components, especially to forms for submission.
