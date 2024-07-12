# React Hot Toast

[React Hot Toast](https://react-hot-toast.com/docs) is a lightweight and customizable library for adding beautiful, animated toast notifications to React applications.

```
npm install react-hot-toast
```

```jsx
import toast, { Toaster } from 'react-hot-toast';

function App() {
  const notify = notification => toast(notification);

  return (
    <div>
      <Toaster />
      <button onClick={notify}>Show Toast</button>
    </div>
  );
}
```

- The `Toaster` component should be included at the root of the application to handle the rendering of toast notifications.
- The `toast` function is used for triggering toast notifications.

[Options for Customization](https://react-hot-toast.com/docs/toaster)
