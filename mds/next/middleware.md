# Middleware in Next.js

**Middleware:** function that executes between a incoming request and the response.

**Next.js Middleware:** function that runs after the request but before the route accessed by the user is rendered.

This middleware function must to be exported from the `middleware.js` file which must be placed at the root of the project directory, and not in the `app` folder.

```js
// middleware.js
import { NextResponse } from "next/server";

export function middleware(request) {
  // console.log(request)
  return NextResponse.redirect(new Url("/about", request.url));
}

export const config = {
  matcher: ["/account", "/contact"],
};
```

**Middleware Use Cases:** protecting routes based on conditions, reading incoming cookies and headers, setting cookies and headers on the response, this enables us to implement features such as authentication and authorization, server-side analytics, redirects based on geolocation, and many more.

**Matcher:** We should define the routes for which the middleware will execute at `config.matcher`, otherwise this can lead to infinite loops because the middleware function executes on each route redirect by default.

**Note:** The middleware function in Next.js must always produce a response, which can occur in two ways: the most common method is redirecting the user to a specific route, and the second method is sending a JSON response to the client.
