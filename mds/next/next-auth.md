# Auth.js

[**Auth.js**](https://authjs.dev/getting-started/installation) is a library that simplifies authentication in Next.js applications.

## 1. Install next-auth

```
npm install next-auth@beta
```

## 2. Configure a Provider

Configure a google project at `http://console.cloud.google.com`

## 3. Environment Variables

Create a `.env.local` file and define the following environment variables:

`NEXTAUTH_URL`, `NEXTAUTH_SECRET`, `AUTH_GOOGLE_ID`, `AUTH_GOOGLE_SECRET`.

## 4. `auth.js`

Create and setup the `auth.js` file at the root of the project directory. This file contains the `NextAuth` function that is responsible for implementing the authentication.

```js
import NextAuth from "next-auth";
import Google from "next-auth/providers/google";
import { getUser, createUser } from "data-service";

const authConfig = {
  providers: [Google],
  callbacks: {
    authorized({ auth, request }) {
      return !!auth?.user;
    },
    async signIn({ user }) {
      try {
        const existingUser = await getUser(user.email);
        if (!existingUser) await createUser({ email: user.email });
        return true;
      } catch (err) {
        return false;
      }
    },
    async session({ session }) {
      const user = await getUser(session.user.email);
      session.user.userId = user.id;
      return session;
    },
  },
  pages: {
    signIn: "/login",
  },
};

export const { auth, handlers, signIn, signOut } = NextAuth(authConfig);
```

## 4. Route Handlers

`app/api/auth/[...nextauth]/route.js`

```js
import { handlers } from "@/auth";
export const { GET, POST } = handlers;
```

## 5. Redirects

`middleware.js`

```js
import { auth } from "@/auth";

export const middleware = auth;

export const config = {
  matcher: ["/account"],
};
```

When accessing the `/account` route, the middleware function will call the `authConfig.callbacks.authorized` function and check if the user is authenticated or not so that the redirect wil take place.

## 6. SignIn and SignOut Actions

Server actions for sign in and sign out.

```js
// actions.js

import { signIn, signOut } from "@/auth";

export async function signInAction() {
  await signIn("google", { redirectTo: "/account" });
}

export async function signOutAction() {
  await signOut({ redirectTo: "/" });
}
```

- `signInAction` when called, the user will attempt to login using the configured provider.
- `signOutAction` when called, the user will be signed out.

## 7. Using the `auth` function

```js
import { auth } from "@/auth";

export default async function Page() {
  const session = await auth();
  console.log(session);
}
```

## Terminology

**Authentication:** getting the right information about the current user and making sure that the user is who they claim to be.

**Authorization:** allow access to certain areas of the application to users that are logged in and have the right privilege to visit that part.
