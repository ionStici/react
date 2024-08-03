# The Image Component

## Table of Contents

- [The Next.js `Image` Component](#the-nextjs-image-component)
- [Statically Imported Images](#statically-imported-images)
- [The fill prop](#the-fill-prop)

## The Next.js `Image` Component

```jsx
import Image from "next/image";

export default function Page() {
  return <Image src="logo.png" alt="Logo" width="50" height="50" />;
}
```

_What the `Image` component does:_

1. It automatically serves correctly sized images in modern formats, such as WebP, and only when necessary.
2. It prevents layout shifts, it forces us to specify the exact width and height of the image.
3. It automatically lazy loads images, so when when the image enters the viewport.

## Statically Imported Images

When we serve a statically imported image to the `Image` component, Next.js will be able to analyze the image first, and we don't need to specify the width and height attributes.

```js
import Image from "next/image";
import logo from "@/public/logo.png";

export default function Page() {
  return <Image src={logo} alt="Logo" priority={true} />;
}
```

_In this scenario we can add a few more properties:_

- `priority` : Should only be used when the image is visible above the fold. Defaults to 'false'.
- `quality` : The quality of the image defined in percentages.
- `placeholder="blur"` : will make the image blur at loading.

## The fill prop

```jsx
import Image from "next/image";

export default function Page() {
  return (
    <div className="relative aspect-square">
      <Image src="bg.jpg" fill class="object-cover object-top" />
    </div>
  );
}
```

- `fill` : this prop will make the image fill the entire relative containing block.
- `object-fit: cover;` and `object-position: top;` complement this technique.

`fill` allows us to use static images without specifying width and height.

## Notes

If the image `src` comes from an api, you have to specify that in the [`next.config.mjs`](https://nextjs.org/docs/messages/next-image-unconfigured-host) file.
