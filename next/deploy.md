# Next.js Blog Project Notes

<!-- Note: markdown metadata & yaml format -->

NextJS has built-in lazy loading for page routes. Code for different pages is fetched on demand when we visit a page.

## Building NextJS Apps: Options

2 different ways of building and deploying next.js app:

1. **Standard build** - `"build": "next build"` - produces & spits an optimized production bundle, AND a server-side app (requires NodeJs server to run it, we can't use run this server-side build using a static host). Why it needs a NodeJS Server: because of the built-in server-side features, pre-rendering pages on the fly on the server, revalidating pages, API routes.

   Pages are pre-rendered, but NodeJS server is required for API routes, server-side pages and page revalidations, dynamic pages for `getStaticPaths`.

   Re-deply needed if code changes or you don't use revalidations and need page updates.

2. **Full Static Build** - `"export": "next export"` - produces an optimizaed production bundle of our app as well, BUT produces 100% static app (HTML, CSS, JS): No NodeJS server required. This means that you can't use certain NextJS features: API routes, server-side pages, page revalidations, static paths with fallbacks set to true or blocking.

   Re-deploy needed for code and content changes.

## Key Deployment Steps

1. Add page metadata, optimize code, remove unnecessary dependencies

2. Use environment variables for variable data (e.g. database credentials, API keys, etc.)

3. Do a test build and test the production-ready app locally or on some test server

4. Deploy

## 2. The NextJS Config File & Working With Environment Variables

Environment varialbes - use different values in different parts of our code during development and production. NextJS has built-in support for environment variables.

We can configure nextJS:

```js
// next.config.js root file

module.exports = {
    module.exports = {
  env: {
    mongodb_username: 'un',
    mongodb_password: 'pw',
    mongodb_clustername: 'cluster0',
    mongodb_database: 'my-site',
  },
};

};
```

We can use our environment variables anywhere in our codebase.

We access these variable through the global `process` object exposed by nodeJS, then on this object we can find the `eng` object that contains our variables.

The variables are replaced with their values during the build process.

```js
process.eng.mongodb_username;
```

With NextJS, we can define different sets of configurations values for development and production. We can import in the config file different phases modules:

```js
const { PHASE_DEVELOPMENT_SERVER } = require("next/constants");

module.exports = (phase) => {
  if (phase === PHASE_DEVELOPMENT_SERVER) {
    return {
      eng: {
        env: {
          mongodb_username: "un",
          mongodb_password: "pw",
          mongodb_clustername: "cluster0",
          mongodb_database: "my-site-dev",
        },
      },
    };
  }

  return {
    env: {
      mongodb_username: "un",
      mongodb_password: "pw",
      mongodb_clustername: "cluster0",
      mongodb_database: "my-site",
    },
  };
};
```

Using the `if` statement we check if we are in the development or production step.

NextJS will run this function and pass in an `phase` object that indicates the phase we are in.

If we make past first `if` check, we are not in the dev phase.

## Running a Test Build & Reducing Code Size

`npm run build`

In the terminal we have information about the build.

## A Full Deployment Example (To Vercel)

The `.next` folder contains the production bundle.

Hosting provider that supports NodeJS.

When deploying a NextJS app -> we push the code to a remote repository like GitHub -> we can connect the remote repository to the Vercel acc and tell vercel that whenever the code from that repo changes, then redeploy.

After importing the project to vercel from github..

It will take and put our api routes into serverless functions which are executed on demand.

## the "export" Feature

1. `next build`
2. `next export`

After running both commands, we get a `out` folder we our static website.
