# NextAuth

NextAuth is a library that simplifies authentication in Next.js applications. This library is gonna make it easy to connect to many different auth providers. And we can also use our own users from our own database.

```
npm install next-auth@beta
```

**Authentication:** getting the right information about the current user and making sure that the user is who they claim to be.

**Authorization:** allow access to certain areas of the application to users that are logged in and have the right privilege to visit that part.

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
