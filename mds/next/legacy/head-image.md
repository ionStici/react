# Optimizing NextJS Apps

## Configuring the "head" Content

Add `<head>` metadata using `Head` next.js component.

We can add this `Head` special component anywhere in our JSX code for a given component.

Next.js will inject the content of the `Head` component into the `<head>` section of the page.

```js
import Head from "next/jead";

function HomePage() {
  return (
    <div>
      <Head>
        <meta name="description" content={event.description} />
        <title>Hello World!</title>
      </Head>
      <main></main>
    </div>
  );
}
```

We can inject dynamic content inside `Head` as usual.

## Reusing Logic Inside A Component

In case a component has multiple checks using the `return` statement..

```jsx
function HomePage() {
  const pageHeadData = (
    <Head>
      <title>Hello World!</title>
    </Head>
  );

  if (!firstCheck) {
    return (
      <>
        {pageHeadData}
        <div></div>
      </>
    );
  }

  if (true) {
    return (
      <>
        {pageHeadData}
        <div>content</div>
      </>
    );
  }
}
```

Standard React way of re-using logic.

## Working with the `"_app.js"` File (and Why)

Head settings that are the same across all page components.

`_app.js` is the root app component, which in the end is rendered for every page that is deing displayed.

This component `<Component {...pageProps} />` which is received through `props` in `MyApp` component, is the actual page component that should be rendered.

Again, the `MyApp` component is automatically rendered by Next.js, then the `Component` prop is automatically set by Next.js, so you don't need to do anything for that.

We can use this `_app.js` file such that we wrap the page component is a wrapper component, to give all pages the same layout, for example the same navigation bar.

```js
// _app.js

function MyApp({ Component, pageProps }) {
  return (
    <>
      <Head>
        <meta name="viewport" content="initial-scale=1.0, width-device-width" />
      </Head>
      <Component {...pageProps} />
    </>
  );
}
```

As well, we can use `Head` component here to apply some generic setting that applies to all our page components.

## Merging "head" Content

Next.js automatically merges multiple `Head` elements.

If we have conflicts, then next.js will simply take the latest `Head` setting in case we have it multiple times.

If we have a general `<title>` in the `_app.js` file, it will be overwritten in situations where we navigate to page components that have their own `<title>` set.

## The `_document.js` File

`_document.js` file must be added in the root `pages` folder.

It's not here by default, but if we add it then next.js will take it in account.

We can imagine `_app.js` as the root component inside of the body section of your html document.

But, `_document.js` allows you to customize the entire html document, all the elements that make up an html document.

_Here, we add a special `class` component:_

```js
import Document, { Html, Head, Main, NextScript } from "next/document";

class MyDocument extends Document {
  render() {
    return (
      <Html lang="en">
        <Head />
        <body>
          <div id="overlays" />
          <Main />
          <NextScript />
        </body>
      </Html>
    );
  }
}

export default MyDocument;
```

It must be a `class` component because it must extends other component provided by next.js

The `Head` component we import here is not the same as the one we import in other page components.

This is a default structure that we should add.

We can use this component in case we want to override the default document structre, for example if we want to add the `lang` attribute to the `html` element.

The `div` we added here, this allows us to add html content outside of our application component tree, for example for using those elements with react portals, we could select this `div` with a react portal to portal our modals.

## Optimizing Images with the "Next Image" Component & Feature

Huge images & jpeg format - not good for production.

[Opi](https://nextjs.org/docs/pages/building-your-application/optimizing/images) / [next/image](https://nextjs.org/docs/pages/api-reference/components/image)

**Optimizing Images with Next.js**

import the next.js `Image` component. With this component, next.js will create multiple versions of our image on the fly when requests are coming in, optimized for the operating systems and device sizes that are making the request, and then those generated images will be cached for future reqeusts from similar devices.

```js
import Image from "next/image";

function Comp() {
  return (
    <>
      <div>
        <Image src={data.image} alt={data.title} width={250} height={160} />
      </div>
    </>
  );
}
```

We need to include the `width` and `height` attributes to inform next.js about the width and height of the image, not the original image, but the image width and height which we need to be rendered.

With `Image` component, in Network DevTools tab we see that instead of 3mb, next.js delivers a image with only 10kb, and instead of a jpeg image now its a webp format.

We can see the generated images in the `.next/cache/images` folder - here we have encrypted folders that contains the images as they are generated by next.js - optimized and generated when needed, not in advanced, only when a request reaches the page, but then they are stored so that future requests from similar devices immediately get that already generated image.

Another benefit - when we increase the screen size, a new request is made. By default, images are lazy laoded (if not visible, next.js will not download them).

`next/image` allows us to ship production-ready images
