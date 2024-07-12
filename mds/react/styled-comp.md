# Styled Components

[Styled Components](https://styled-components.com/) is a CSS-in-JS library that provides component-level styles.

```bash
npm install styled-components
```

```jsx
import { BiLoaderAlt } from "react-icons/bi";

import styled, { keyframes } from "styled-components";

const rotate = keyframes`  to {
    transform: rotate(1turn)
  }`;

const SpinnerMini = styled(BiLoaderAlt)`
  width: 2.4rem;
  height: 2.4rem;
  animation: ${rotate} 1.5s infinite linear;
`;

export default SpinnerMini;
```

## Table of Contents

- [The `styled` Function](#the-styled-function)
- [Global Styles](#global-styles)
- [Conditional Styling](#conditional-styling)
- [`css` helper function](#css-helper-function)
- [Extending Styles](#extending-styles)
- [Variations](#variations)
- [The `attrs` Function](#the-attrs-function)

## The `styled` Function

The `styled` function is used to create styled React components by chaining the name of the HTML element you want to style. Styled components can accept all the same props as regular JSX elements.

```jsx
import styled from "styled-components";

const Button = styled.button`
  font-size: 30px;
`;

const App = () => <Button onClick={() => ""}>Click</Button>;
```

You can use `&` nested selectors within styled components.

**Note:** Avoid defining your styled components within the render logic of a React component.

## Global Styles

```js
// styles/GlobalStyles.js
import { createGlobalStyle } from "styled-components";

const GlobalStyles = createGlobalStyle`
* {
  margin: 0;
  padding: 0;
}
`;

export default GlobalStyles;
```

```jsx
// App.jsx
import GlobalStyles from "./styles/GlobalStyles";

export default function App() {
  return (
    <div>
      <GlobalStyles />
      <Components />
    </div>
  );
}
```

## Conditional Styling

Conditionally apply styles based on `props` passed to the component.

```jsx
const Heading = styled.h1`
  ${(props) =>
    props.as === "h2" &&
    css`
      font-size: 30px;
    `}
`;

const App = () => <Heading as="h2" />;
```

The `as` prop is a special prop that allows you to change the underlying HTML element of a styled component dynamically.

## `css` helper function

The [`css`](https://styled-components.com/docs/api#css) helper function allows you to define a block of CSS that can be reused throughout your styled components.

```js
import styled, { css } from "styled-components";

const buttonStyles = css`
  border: none;
  font-family: inherit;
`;

const Button = styled.button`
  ${buttonStyles}
  border-radius: 5px;
`;
```

Useful for sharing styles between multiple components or for conditional styling.

## Extending Styles

Extending Styles in styled-components allows you to build upon existing styled components by creating new ones that inherit and add to the original component's styles, promoting code reuse and consistency.

```jsx
const Button = styled.button`
  font-size: 30px;
`;

const RedButton = styled(Button)`
  color: red;
`;

const App = () => <RedButton>Click</RedButton>;
```

## Variations

_Code Example_

```jsx
import styled, { css } from "styled-components";

// Variations
const sizes = {
  small: css`
    font-size: 12px;
  `,
  medium: css`
    font-size: 14px;
  `,
  large: css`
    font-size: 16px;
  `,
};

// Dynamically apply styles based on the `size` prop
const Text = styled.p`
  color: #333;
  ${(props) => sizes[props.$size]}
`;

// Set a default variation
Text.defaultProps = {
  $size: "medium",
};

const App = () => <Text $size="large">Hello</Text>;
```

## The `attrs` Function

The `attrs` function allows you to add implicit attributes to your styled components.

```jsx
import styled from "styled-components";

const Input = styled.input.attrs({
  type: "text",
})`
  font-size: 20px;
`;

const App = () => <Input />;
```

By chaining `attrs` to `styled.input`, you can define static or dynamic attributes for the element. These attributes can be set using either a plain object or a function that returns an object.
