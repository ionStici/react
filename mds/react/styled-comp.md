# Styled Components

[Styled Components](https://styled-components.com/) is a CSS-in-JS library that provides component-level styles.

```bash
npm install styled-components
```

## Table of Contents

- [The `styled` Function](#the-styled-function)
- [Global Styles](#global-styles)
- [Conditional Styling](#conditional-styling)
- [`css` helper function](#css-helper-function)
- [Extending Styles](#extending-styles)
- [Variations](#variations)

## The `styled` Function

After the `styled` function, we chain the name of the HTML element we want to style, which returns a styled React component.

These styled components can receive all the same props that regular JSX elements can receive.

```jsx
import styled from 'styled-components';

const StyledApp = styled.div`
  padding: 20px;
`;

const Button = styled.button`
  color: #ef233c;
  font-size: 30px;

  &:hover {
    color: #d90429;
  }
`;

const App = () => (
  <StyledApp>
    <Button onClick={() => ''}>Click</Button>;
  </StyledApp>
);
```

Styled components support nested selectors, so you can use `&:hover {}` within them to define styles for different states.

**Note:** Don't define your styled components inside the render logic of a React component.

## Global Styles

```js
// styles/GlobalStyles.js
import { createGlobalStyle } from 'styled-components';

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
import GlobalStyles from './styles/GlobalStyles';

function App() {
  return (
    <>
      <GlobalStyles />
      <main></main>
    </>
  );
}
```

## Conditional Styling

Conditionally apply styles based on props passed to the component.

```jsx
const Heading = styled.h1`
  ${props =>
    props.as === 'h1' &&
    css`
      font-size: 30px;
      font-weight: 600;
    `}

  ${props =>
    props.as === 'h2' &&
    css`
      font-size: 20px;
      font-weight: 500;
    `}
`;

function App() {
  return (
    <div>
      <Heading as="h1" />
      <Heading as="h2" />
    </div>
  );
}
```

The `as` prop is a special prop that allows you to change the underlying HTML element of a styled component dynamically.

## `css` helper function

The [`css`](https://styled-components.com/docs/api#css) helper function allows you to define a block of CSS that can be reused throughout your styled components.

```js
import styled, { css } from 'styled-components';

const buttonStyles = css`
  border: none;
  font-family: inherit;
  color: inherit;
`;

const Button = styled.button`
  ${buttonStyles}
  border-radius: 5px;
`;
```

Useful for sharing styles between multiple components or for conditional styling.

## Extending Styles

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

```jsx
import styled, { css } from 'styled-components';

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
  ${props => sizes[props.size]}
`;

// Set a default variation
Text.defaultProps = {
  size: 'medium',
};

const App = () => <Text size="large">Hello</Text>;
```
