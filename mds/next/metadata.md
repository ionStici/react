# Metadata

```jsx
// app/contact/page.js

export const metadata = {
  title: "Contact Us",
};

export default function Page() {
  return <h1>Contact Us</h1>;
}
```

Each route segment can have its own metadata, which will override the default metadata from the root layout.

## Favicon

To add a favicon to next.js application all you have to do is to add the icon file to the root of the `app` folder. The file needs to have the name of `icon.*`, the extension doesn't matter.

## Dynamic Metadata

```jsx
export async function generateMetadata({ params }) {
  const { name } = await fetchData(params.id);
  return { title: `Page ${name}` };
}

export default function Page({ params }) {
  return null;
}
```
