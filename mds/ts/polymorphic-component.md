# Polymorphic Component

A **polymorphic component** can render different UIs depending on the `as` prop. This allows the component to be highly flexible and reusable across different contexts.

```tsx
import {
  type ComponentPropsWithoutRef,
  type ElementType,
  type ReactNode,
} from "react";

type ContainerProps<T extends ElementType> = {
  as?: T;
  children: ReactNode;
} & ComponentPropsWithoutRef<T>;

export default function Container<C extends ElementType>({
  children,
  as,
  ...props
}: ContainerProps<C>) {
  const Component = as || "div";

  return <Component {...props}>{children}</Component>;
}
```

- `ElementType` is a generic type provided by React that represents any valid HTML element or React component.

- `T extends ElementType` : `T` is a generic type parameter constrained to `ElementType`, meaning it can be any valid HTML tag or React component.

- `as?: T` : The `as` prop allows you to specify the type of element or component that the `Container` should render. This prop is optional (`?`), and if not provided, the component will default to rendering a `div`.

- `ComponentPropsWithoutRef<T>` : This combines the custom `ContainerProps` with all the standard props that the specified `T` element can receive. For example, if `T` is a `"button"`, then props like `onClick` and `disabled` are included.

- `C extends ElementType` : The Container component itself is defined as a generic component with `C` representing the type of element or component that will be rendered.

- The `as` prop specifies what type of element or component to render.

- `...props` rest operator gathers any additional props that are passed to the component.

- `const Component = as || "div"` : This line sets `Component` to be the element or component specified by `as`. If `as` is not provided, it defaults to `"div"`.
