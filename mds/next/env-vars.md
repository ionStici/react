# Environment Variables

**Environment variables** are used to store configuration settings and other information that can change depending on the environment where the code is running (e.g., development, testing, production). They help keep sensitive data (like API keys) secure and make your code more flexible.

**Definition:** Environment variables are key-value pairs. The key is a variable name, and the value is the value assigned to that variable.

## Using Environment Variables in a Next.js App

Next.js supports [Environment Variables](https://nextjs.org/docs/app/building-your-application/configuring/environment-variables) out of the box. Here's how you can use them:

### 1. Creating Environment Variables

In Next.js, you can define environment variables in special files in the root of your project:

- `.env` : Loaded for all environments.
- `.env.local` : Loaded for all environments except for `test`.
- `.env.development` : Loaded in `development`.
- `.env.production` : Loaded in `production`.
- `.env.test` : Loaded in `test`.

#### Example:

```env
# .env.local
NEXT_PUBLIC_API_URL=https://api.example.com
SECRET_API_KEY=your-secret-key
```

### 2. Accessing Environment Variables

Next.js provides the `process.env` object to access environment variables.

**Client-Side:** To use an environment variable in client-side code, prefix the variable with `NEXT_PUBLIC_`. Next.js will only expose variables that start with this prefix to the client.

```js
// Client-side usage
const apiUrl = process.env.NEXT_PUBLIC_API_URL;
```

**Server-Side:** You can use any environment variable directly in server-side code.

```js
// Server-side usage
const secretApiKey = process.env.SECRET_API_KEY;
```

### 3. Adding Environment Variables in Build/Deployment

When deploying your Next.js app, make sure to set the environment variables in your deployment platform (e.g., Vercel, Netlify, or your own server).

- **Vercel:** Go to the project settings, then the "Environment Variables" section, and add your variables there.
- **Netlify:** Go to the site settings, then "Build & Deploy" -> "Environment" and add your variables.

This approach ensures that your environment-specific configuration stays outside your codebase, making your application more secure and easier to manage.
