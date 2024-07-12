# React Hot Toast

[React Hot Toast](https://react-hot-toast.com/docs) is a lightweight and customizable library for adding beautiful, animated toast notifications to React applications.

```
npm install react-hot-toast
```

```jsx
import toast, { Toaster } from "react-hot-toast";

function App() {
  const notify = (text = "Hello") => toast(text);

  return (
    <div>
      <Toaster />
      <button onClick={notify}>Show Toast</button>
    </div>
  );
}
```

- The [`Toaster`](https://react-hot-toast.com/docs/toaster) component should be included at the root of the application to handle the rendering of toast notifications.

- The [`toast`](https://react-hot-toast.com/docs/toast) function is used for triggering toast notifications.

## Customization

```jsx
<Toaster
  position="top-center" // the position of toasts
  gutter={8} // gap between toasts
  containerStyle={{}}
  containerClassName="toastContainer"
  toastOptions={{
    // Default Options:
    duration: 5000,
    className: "",
    style: { background: "#363636", color: "#fff" },
    // Default Options for specific types:
    success: {},
    error: {},
  }}
/>
```

The `toast` function can receive an options object as a second argument that will override all the default options received from `Toaster`.
