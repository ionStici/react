# Loading and Optimizing Fonts

The `next/font` package provides built-in support for Google Fonts and other font sources.

```jsx
// app.layout.js

import { Inter } from "next/font/google";

const inter = Inter({
  subsets: ["latin"],
  display: "swap",
  // weight: ["400", "700"], style: ["normal", "italic"],
});

export default function RootLayout({ children }) {
  return (
    <html className={`${inter.className}`}>
      <body>{children}</body>
    </html>
  );
}
```

The `Inter` function is imported and called with a config object. This function handles the loading of the font files.

The `inter` variable holds the `className` property which can be used to apply the font.

## Config options

- `subsets` : Specifies the character sets to include, such as "latin".
- `display` : Defines how the font should be displayed while loading; "swap" ensures that text remains visible during the font load by using a fallback font initially.
- `weight` : Specifies the font weights to be loaded, such as "400" and "700".
- `style` : Defines the styles to be included, such as "normal" and "italic".

If using a variable font, you don't have to specify the font weight.

## Variable Fonts vs Static Fonts

Use [**Variable Fonts**](https://fonts.google.com/variablefonts) for better performance and flexibility.

- **Variable Fonts** maintain a single file having all the possible variations.
- **Static Fonts** maintain a different file for each variation.
