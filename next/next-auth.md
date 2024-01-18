# Authentication

## How Does Authentication Work (In React & NextJS Apps)?

Client (Browser) <---> Server (connected to a database)

After filling the "login" form, we send a request to the server with the user data. On the server we validate that input and we send back a response with yes/no. If the user was successfully authenticated, he can access protected routes, send requests to other protected endpoints to the server. The authentication to be trustworthy we need "proof", and for this we got 2 main mechanisms:

1. **Server-side Sessions** - store unique identifier on the server, and the same identifier to the client that sent us the credentials. The client sends identifier along with requests to protected resources. With SSL the identifier can't be stoled. On the client it is stored on a cookie typically, that cookie can be configured such that it's not accessible through JavaScript, to prevent cross-site-script attacks, so that it's readable only to the server when requests are made. If we protect from cross site scripting (XSS) then the client is pretty secure, only the user and server have access.

2. **Authentication Tokens** - no identifiers, instead the server creates and asigns (but not store) "permission" tokens (random strings) on the server, and sends a token to the client. These random strings (tokens) can be unpacked to data packages. The client saves this token and asigns it to ongoing requests to protected resources. The server doesn't store this token anywhere, still the server knows how it signed that token, so the server will be able to verify if the token was created by it or not.

When building SPAs, we typically work with Tokens instead of Sessions. Why? Pages are served directly and populated with logic without hitting the server, we don't send a request for every page you visit, the server doesn't see every request you have send, therefore you load pages without the server being able to directly find out if you are authenticated ot not.

Another reason for SPAs + Tokens: Backend APIs which are used for SPAs are typically stateless, they don't care about the individual connected clients, they don't keep track of all the connected clients, instead that API can work pretty much on its own, it just is able ot hand out permissions to clients who authenticated so that they can then later request access to protected resources. The API itself does not store any extra information about any connected clients.

For SPAs, the server is not involved in every request and every action that's happening on our page, because we handled that with frontend JavaScript, and we have that stateless API connected to the SPA.

This results in a "detached" forntend and backend combination, they do talk to each other sometimes, but not for every action that's going on on the page.

- Servers don't save information about authenticated clients.
- Instead, clients should get that permission which allows them to prove that they are authenticated.

That's why we use tokens (JWT: JSON Web Tokens).

### JSON Web Tokens (JWT)

JWT is generated with 3 main building blocks.

- Issuer Data (data "metadata" automatically added into a token by the server when that token is generated, preconfigured by 3rd party packages we use for generating tokens).
- Custom Data (e.g. user data)
- Secret Signing Key - we set this on the server, the client never gets to see this key.

We generate a token with some third-party package by combining all these pieces together, by creating a long random string that incorporates all that data and is signed by that key.

Server-side Token Creating + Signing = JSON Web Token.

"Signing" does not mean encryption, JWT is not encrypted, you can unpack it and read the data inside of it without knowing that key, that key only proves that a given server created that token, but you can read the content of the token without knowing that key, the key will not be included in the token, you can't read the key even if unpack the token.

So it's that token that is stored by the client, which then is attached to requests to protected resources on the server, for example if you want to change the password, we don't just send the old and new password, but we also include that token in the outgoing request.

That token then is validated by the server, by checking if it could generate that token using the key that only it has. So it can omly generate the token by knowing the key.

For token creation and validation we can rely on secure third-party packages

<br>

## The "next-auth" Library

[NextAuth.js](https://next-auth.js.org/)

`next-auth` package - implementing auth to nextjs apps.. Add auth to nextjs apps. Supports broad variety of authentication providers: email and password & add sign in with google or facebook or apple.

https://next-auth.js.org/providers/

We will use: our own email and password combination stored in a database.

next-auth has Server-side + Client-side capabilities. Verify if the user is logged in on api routes, use in components. It will also helps Generate tokens.

We need to build User Creations logic (sign up & user verification).

We will store user data in collections in MongoDB.

### bcryptjs

Don't store plain passwords in the database, instead encrypt them.

```
npm install bcryptjs
```

`bcryptjs` is a package that encrypt passwords or any kind of data.

```js
import { hash, compare } from "bcryptjs";

export async function hashPassword(password) {
  const hashedPassword = await hash(password, 12);
  return hashedPassword;
}

export async function verifyPassword(password, hashedPassword) {
  const isValid = await compare(password, hashedPassword);
  return isValid;
}
```

The second argument of `hash()` is a number that dictates how strong the encsyption is. `hash` returns a promise.

## Adding the "Credentials Auth Provider" & User Login Logic

next-auth heps with authenticating users, finding out whether a user thas that permission, by managing that token creation and storage behind the scenes.

To use next-auth, we start by adding another api route, because logging a user in requires a request to a certain api route where we look into the database and find out whether we have that user in the db.

`pages/api/auth/[...nextauth.js]`

We need this `[...nextauth].js` catch-all route, because the next-auth pacakge behind the scenes will expose multiple routes for user login and for user logout, so we use a catch-all route so that all those special requests to these special routes are automatically handled by the next-auth package. We can check what routes next-auth creates by taking a look at the _Documentation/REST API_ on the official website.

```js
// /api/auth/[...nextauth].js
import NextAuth from "next-auth/next";
import CredentialsProvider from "next-auth/providers/credentials";

export default NextAuth({
  session: {
    jwt: true,
  },

  providers: [
    CredentialsProvider({
      async authorize(credentials) {},
    }),
  ],
});
```

Here we import, export and execute the `NextAuth` function. When we execute it, it will return a handler function created by NextAuth. When calling it, we can pass a configuration object to it, to configure the next-auth behavior.

The imported `NextAuth` is an API route, that's why it is a function.

The `authorize` async method will be called by next-auth when it receives an incoming login request, as argument we get the `credentials` that were submitted. In this function we bring our own authorization logic. We get an email through `credentials` object, and then we check if we find that email in the db.

If we throw an error, `authorize` will reject the promise it generates, and by default will redirect the client to another page, we need to override this to stay on the login page.

The object returned by `authorize` (email, not password) will be encoded in the JSON Web Token.

To make sure that a JWT is created, inside `NextAuth` include the option: `session: { jwt: true }`

The `session` object configures how the session for an authenticated user will be managed.

## Sending a "Signin" Request From The Frontend

```js
import { signIn } from "next-auth/react";

function AuthForm() {
  //

  async function submit() {
    const result = await signIn("credentials", {
      redirect: false,
      email: enteredEmail,
      password: enteredPassword,
    });
  }

  //
}
```

- **First** argument describes the provider with whichw e wanna sign in.

- **Second** argument is a configuration object for how the sign in process should work.

The `redirect: false` property will override the default which redirects the user to another page after an error.

When setting `redirect: false`, `signIn` will return a promise with our result (never rejected).

This second argument object of `signIn` is received in the `authorize` function as `credentials` argument.

So, we send email and password credential data to the backend.

With this we can sign in users, and in the `result` object we get the yes / no response.

## Managing Active Session (On The Frontend)

Whether a user is or not authenticated.

After we log in successfully, nextjs adds a cookie - DevTooks > Application > Cookies > Domain Name

These are cookies generated and managed by nextjs, the next-auth.session-token is the JWT

It will use this token automatically for us when we try to change what we see on the screen or when we try to send requests to protected resources.

For example we can find our if a user is logged in or not by finding out if whether such a valid token exists.

```js
import { useSession } from "next-auth/react";

function Comp() {
  const { data: session, status } = useSession();
  const loading = status === "loading";

  console.log(loading); // true -> false
  console.log(session); // udefined -> User data
}
```

To use `useSession`, wrap your app in `<SessionProvider>`, check the docs for more info.

We can use the `useSession` hook to find out if a user is logged in. we can use it in any functional react component. It returns 2 properties, `session` describes the actual session, `loading` tells us wherever nextjs is currently still figuring out whethere we are logged in or not.

`loading` is `true` which means that nextjs is finding out whether the user is authenticated or not, also at the same time `session` is `undefined` because of the same reason, then `loading` becomes `false` which means that nextjs is not trying to log in anymore, and then `session` becomes an object with user data from the encoded token + expiration data for when the session wil expire.

Using this data from `useSession` hook we can update the user interface accordingly if the user is or not logged in.

## Adding User Logout

```js
import { signOut } from "next-auth/react";

function logoutHandler() {
  signOut();
}
```

With `signOut()` function, nextjs will clear the session-token cookie, then `useSession` will update the session.

## Adding Client-Side Page Guards (Route Protection)

Redirect away if auth or not.

We can't use `useSession` because it doesn't update. Instead:

`getSession` hook sends a new request and gets the latest session data, then we can react to the response for that request.

Page loading briefly UI element - because we have that brief moment where we don't know yet whether we are authenticated or not.

```js
// user-profile.js component
import { getSession } from "next-auth/react";

function UserProfile() {
  const [isLoading, setIsLoading] = useState(true);

  useEffect(() => {
    getSession().then((session) => {
      if (!session) {
        window.location.href = "/auth";
      } else {
        setIsLoading(false);
      }
    });
  }, []);

  if (isLoading) {
    return <p className={classes.profile}>Loading...</p>;
  }

  return <JSX />;
}
```

## Adding Server-Side Page Guards (And When To Use Which Approach)

Use server-side code to determine whether the user is authenticated or not and return different page content and possibly a redirect if the user is not authenticated.

```js
// pages/profile.js

import UserProfile from "../components/profile/user-profile";
import { getSession } from "next-auth/react";

export default function ProfilePage() {
  return <UserProfile />;
}

export async function getServerSideProps(context) {
  const session = await getSession({ req: context.req });

  if (!session) {
    return {
      redirect: {
        destination: "/auth",
        permanent: false,
      },
    };
  }

  return {
    props: { session },
  };
}
```

`getSession()` hook can be used on the server-side code as well, in `getServerSideProps` for example.

Headers, `getSession` will automatically look into that request, extract the data it needs = the session token cookie, to see if that's valid and if the user is authenticated, all behind the scenes.

`session` will be null if the user is not authentificated, and will be a valid session object if the user is authenticated.

Note: getServerSideProps is basically a function that runs code on the server side to then pass props to the page component.

This will redirect without even flashing the user interface.

Now, the `user-profile.js` & `UserProfile` component will only render if the `pages/profile.js` page will render accoridng to the result from `getServerSideProps` function logic.

<!--
## Protecting the "Auth" Page

Redirect after loggin successfuly.
-->

## Using the "next-auth" Session Provider Component

```js
// _app.js
import { SessionProvider } from "next-auth/react";

export default function MyApp({ Component, pageProps }) {
  return (
    <SessionProvider session={pageProps.session}>
      <Layout>
        <Component {...pageProps} />
      </Layout>
    </SessionProvider>
  );
}
```

By passing `sesstion={pageProps.session}` to `SessionProvider`, which it gets from `getServerSideProps` that was returned, this allows next-auth to skip the extra session check performed by `useSession` if we already have the session from `getServerSideProps` function.

If `pageProps.session` is undefined, so it's not set, then `setSession` will check it manually.

This can save some performance and avoid some redundant HTTP requests.

Using `SessionProvider` wrapper is a recommended next-auth optimization.

Session = JSON Web Token (in our case), that is managed by next-auth, JWT is stored by next-auth in our browser cookie, next-auth determines whether we have an active session by checking that cookie & that token from that cookie.

## Protecting API Routes

```js
// pages/api/change-password.js
import { getSession } from "next-auth/react";

export default async function handler(req, res) {
  if (req.method !== "PATCH") return;

  const session = await getSession({ req: req });
  if (!session) return;
}
```

This is all the code needed to protect an api route.

HTTP request methods that make sense for changing a password: POST, PUT, PATCH - these methods imply that resources on the server should be created or changed.

Again, on the server-side, we can pass the request object to `getSession({ req: req })` - it needs this request object because it will then look into it, see if a session token cookie is part of the request, if it is it will validate and extract that data from that cookie, then give us the session object.

401 code - authentication is missing

403 - you are authentcated, but you are not authorized (permission) for this operation, in our case because passwords ar enot equal.

422 - user input is not correct.

## Adding the "Change Password" Logic

We encoded the email address in our token.

```js
import { connectToDatabase } from "../../../lib/db";
import { getSession } from "next-auth/react";
import { hashPassword, verifyPassword } from "../../../lib/auth";

export default async function handler(req, res) {
  if (req.method !== "PATCH") return;

  const session = await getSession({ req: req });
  if (!session) {
    res.status(401).json({ message: "Not authenticated!" });
    return;
  }

  const userEmail = session.user.email;
  const oldPassword = req.body.oldPassword;
  const newPassword = req.body.newPassword;

  const client = await connectToDatabase();

  const usersCollection = client.db.collection("users");

  const user = await usersCollection.findOne({ email: userEmail });

  if (!user) {
    res.status(404).json({ message: "User not found." });
    client.close();
    return;
  }

  const currentPassword = user.password;

  const passwordAreEqual = await verifyPassword(oldPassword, currentPassword);

  if (!passwordAreEqual) {
    res.status(403).json({ message: "Invalid password." });
    client.close();
    return;
  }

  const hashedPassword = await hashPassword(newPassword);

  const result = await usersCollection.updateOne(
    { email: userEmail },
    { $set: { password: hashedPassword } }
  );

  client.close();
  res.status(200).json({ message: "Password updated!" });
}
```
