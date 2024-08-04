# Auth.js

[**Auth.js**](https://authjs.dev/getting-started/installation) is a library that simplifies authentication in Next.js applications.

## 1. Install next-auth

```
npm install next-auth@beta
```

## 2. Configure a Provider

Configure a google project at `http://console.cloud.google.com`

## 3. Environment Variables

Create a `.env.local` file and define the following environment variables: `NEXTAUTH_URL`, `NEXTAUTH_SECRET`, `AUTH_GOOGLE_ID`, `AUTH_GOOGLE_SECRET`

## 4. `auth.js`

Create and setup the `auth.js` file at the root of the app directory.

```js
import NextAuth from "next-auth";
import Google from "next-auth/providers/google";

const authConfig = {
  providers: [Google],
};

export const { auth, handlers, signIn, signOut } = NextAuth(authConfig);
```

<br>
<br>
<br>
<br>
<br>
<br>
<br>
<br>

## Terminology

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
