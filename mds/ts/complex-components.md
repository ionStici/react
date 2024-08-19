# Complex Components

## Table of Contents

- [Flexible Button Component](#flexible-button-component)
- [forwardRef with TypeScript](#forwardref-with-typescript)
- [Form Component with forwardRef and useImperativeHandle](#form-component-with-forwardref-and-useimperativehandle)

## Flexible Button Component

```tsx
import { type ComponentPropsWithoutRef } from "react";

type ButtonProps = ComponentPropsWithoutRef<"button"> & { href?: never };

type AnchorProps = ComponentPropsWithoutRef<"a"> & { href?: string };

function isAnchorProps(props: ButtonProps | AnchorProps): props is AnchorProps {
  return "href" in props;
}

export default function Button(props: ButtonProps | AnchorProps) {
  if (isAnchorProps(props)) {
    return <a className="btn anchor" {...props}></a>;
  }

  return <button className="btn" {...props}></button>;
}
```

```tsx
return <Button>Click</Button>;
```

- `ComponentPropsWithoutRef<'a'>` is a utility type used to extract all standard props for the specified HTML element, except for the `ref` prop. For example, an `'a'` anchor element gives you props such as `href`, `target`, etc.

- `ButtonProps` and `AnchorProps` types define the props for button and anchor elements, ensuring that `href` is only allowed for anchors.

- The `isAnchorProps` function is a custom type guard that checks wether the `props` object contains an `href` property. The `props is AnchorProps` return type tells TypeScript that if this function returns `true`, the `props` parameter can be safely treated as `AnchorProps`.

- By using the `isAnchorProps` type guard, the component can conditionally and safely determine the correct element to render.

## forwardRef with TypeScript

```tsx
import { forwardRef, type ComponentPropsWithoutRef } from "react";

type InputProps = {
  label: string;
  id: string;
} & ComponentPropsWithoutRef<"input">;

const Input = forwardRef<HTMLInputElement, InputProps>(function Input(
  { label, id, ...props },
  ref
) {
  return (
    <div>
      <label htmlFor={id}>{label}</label>
      <input id={id} name={id} ref={ref} {...props} />
    </div>
  );
});

export default Input;
```

- `ComponentPropsWithoutRef<"input">` this utility type extracts all the standard props that can be applied to an `input` element, excluding `ref` prop.

- `forwardRef<HTMLInputElement, InputProps>` : These generic type parameters passed to `forwardRef` specifies the type of the DOM element and the props type the `Input` component accepts.

### Using the Input component

```tsx
import Input from "Input";
import { useRef } from "react";

export default function App() {
  const input = useRef<HTMLInputElement>(null);

  return <Input type="text" label="Name" id="name" ref={input} />;
}
```

## Form Component with forwardRef and useImperativeHandle

```tsx
import {
  type FormEvent,
  type ComponentPropsWithoutRef,
  useRef,
  useImperativeHandle,
  forwardRef,
} from "react";

export type FormHandle = {
  clear: () => void;
};

type FormProps = ComponentPropsWithoutRef<"form"> & {
  onSubmit: (value: unknown) => void;
};

const Form = forwardRef<FormHandle, FormProps>(function Form(
  { onSubmit, children, ...otherProps },
  ref
) {
  const form = useRef<HTMLFormElement>(null);

  useImperativeHandle(ref, () => {
    return {
      clear() {
        form.current?.reset();
      },
    };
  });

  function handleSubmit(event: FormEvent<HTMLFormElement>) {
    event.preventDefault();

    const formData = new FormData(event.currentTarget);
    const data = Object.fromEntries(formData);
    onSubmit(data);
  }

  return (
    <form onSubmit={handleSubmit} {...otherProps} ref={form}>
      {children}
    </form>
  );
});

export default Form;
```

### Using the Form

```tsx
import { useRef } from "react";
import Form, { FormHandle } from "Form";
import Input from "Input";

function App() {
  const form = useRef<FormHandle>(null);

  function handleSubmit(data: unknown) {
    // const extractedData = data as { name: string; age: string }
    // Type narrowing instead:
    if (
      !data ||
      typeof data !== "object" ||
      !("name" in data) ||
      !("age" in data)
    ) {
      return;
    }
  }

  // At this point, TypeScript know that data must
  // be an object with a name and age property,
  // otherwise the previous if statement would have returned
  console.log(data);
  form.current.clear();

  return (
    <form onSubmit={handleSubmit} ref={form}>
      <Input type="text" label="Name" id="name" />
      <Input type="number" label="Age" id="age" />
      <button>Submit</button>
    </form>
  );
}
```
