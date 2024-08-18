# TypeScript with React - Essentials

## Table of Contents

- [Props Types](#props-types)
- [`FC` Type (Functional Component)](#fc-type-functional-component)
- [Types and useState](#types-and-usestate)
- [TypeScript and Forms](#typescript-and-forms)
- [Discriminated Union](#discriminated-union)

## Props Types

```tsx
// Header.tsx
import { type PropsWithChildren, type ReactNode } from "react";

// Props Type Solution 1
type HeaderProps = {
  image: {
    src: string;
    alt: string;
  };
  children: ReactNode;
};

// Props Type Solution 2
type HeaderProps = PropsWithChildren<{
  image: {
    src: string;
    alt: string;
  };
}>;

// Using the Props Type
export default function Header({ image, children }: HeaderProps) {
  return (
    <header>
      <img {...image} />
      {children}
    </header>
  );
}
```

- The `type` keyword in the import statement indicates that it imports a type and not a value of function. This helps TypeScript and some bundlers recognize that these imports are purely for type-checking purposes and will not result in any JavaScript code being included in the final bundle.

- The `type HeaderProps` alias defines the shape of the props the component will receive.

- `children` is typed as `ReactNode`, which is a type provided by React that ensures that `children` can contain any valid React content.

- `PropsWithChildren` is a utility type which automatically adds the `ReactNode` type to the `children` prop.

- `({ image, children }: HeaderProps)` here the `HeaderProps` type is applied to the props object the component receives. This ensures that TypeScript enforces the structure of the props, providing type safety and autocompletion in the editor.

- **Type Safety:** If you pass props that don't match the HeaderProps type, TypeScript will raise an error, helping you catch issues during development.

## `FC` Type (Functional Component)

`FC` is a generic type where the connected type is the props type.

```tsx
import { type FC, type PropsWithChildren } from "react";

type HeaderProps = PropsWithChildren<{ title: string }>;

const Header: FC<HeaderProps> = ({ title, children }) => {};
```

## Types and useState

```tsx
import { useState } from "react";

type Item = {
  text: string;
  id: number;
};

export default function App() {
  const [items, setItems] = useState<Item[]>([]);

  function handleAddItem(text: string): void {
    setItems((prevItems) => [...prevItems, { text, id: Math.random() }]);
  }

  function handleDeleteItem(id: number): void {
    setItems((prevItems) => prevItems.filter((item) => item.id !== id));
  }

  return null;
}
```

`useState < Item[] > ([])` here the `useState` hook is typed with a generic type, which defines the state as an array of `Item` objects (`Item[]`).

## TypeScript and Forms

```tsx
import { type FormEvent, useRef } from "react";

type FormProps = {
  onSubmit: (input: string) => void;
};

export default function Form({ onSubmit }: FormProps) {
  const input = useRef<HTMLInputElement>(null);

  function handleSubmit(event: FormEvent<HTMLFormElement>) {
    event.preventDefault();

    const inputValue = input.current!.value;
    onSubmit(inputValue);
    event.currentTarget.reset();
  }

  return (
    <form onSubmit={handleSubmit}>
      <input type="text" ref={input} />
      <button>Submit</button>
    </form>
  );
}
```

- `type FormEvent` : Specifies the type for the `event` object in the form submission handler, ensuring it is treated as a form event.

- `HTMLFormElement` in `FormEvent<HTMLFormElement>` ensures TypeScript correctly identifies the event target as a form element.

- `useRef<HTMLInputElement>(null)` : The `useRef` hook is typed to reference an HTML input element, providing type safety and autocompletion for DOM operations on the input field.

- The `null` argument initializes `ref` to prevent TypeScript from complaining about potentially accessing properties on `undefined`, ensuring that `ref.current` is either `null` or an `HTMLInputElement`.

- `item.current!.value` : The non-null assertion `!` is used here to tell TypeScript that `item.current` is not `null` when accessing its `value` property.

## Discriminated Union

A discriminated union is a pattern in TypeScript for creating a type that could be one of several types, each distinguished by a common property, known as a "discriminator" or "tag". In the code example below, the `mode` property serves as the discriminator.

```tsx
import { type ReactNode } from "react";

type GreenBoxProps = {
  mode: "green";
  children: ReactNode;
};

type RedBoxProps = {
  mode: "red";
  severity: "low" | "medium" | "high";
  children: ReactNode;
};

type BoxProps = GreenBoxProps | RedBoxProps;

export default function Box(props: BoxProps) {
  const { children, mode } = props;

  if (mode === "green") {
    return (
      <div className="box box--green">
        <p>{children}</p>
      </div>
    );
  }

  const { severity } = props;

  return (
    <div className={`box box--red severity--${severity}`}>
      <p>{children}</p>
    </div>
  );
}
```

- `GreenBoxProps` and `RedBoxProps` are types representing the different possible states of the `Box` component. The `mode` property (`"green"` or `"red"`) acts as a discriminator, allowing TypeScript to differentiate between these two types.

- `BoxProps` is a union of `GreenBoxProps` and `RedBoxProps`. This means that `BoxProps` can be either of these types, but not both at the same time. The `mode` property is used to narrow down the type by determining which type is currently in use.

- When `mode` is `"green"`, TypeScript knows that `props` must be of type `GreenBoxProps`. Therefore, it allows you to safely render the green box without worrying about the `severity` property, which doesn't exist in `GreenBoxProps`.

- If `mode` is not `"green"`, TypeScript infers that `props` must be of type `RedBoxProps`, so it’s safe to destructure `severity` from props.
