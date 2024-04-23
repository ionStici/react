# Styled Components

[Styled Components](https://styled-components.com/) is a CSS-in-JS library that provides component-level styles.

```bash
npm install styled-components
```

## Using styled components

**Note:** Don't define your styled components inside the render logic of a React component.

```jsx
import styled from 'styled-components';

const Button = styled.button`
  font-size: 30px;
`;

const App = () => <Button>Click</Button>;
```

## Conditional styling using props

```jsx
const Button = styled.button`
  color: ${({ color }) => (color === 'red' ? 'red' : 'blue')};
  font-size: 30px;
`;

const App = () => <Button color="red">Click</Button>;
```

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
