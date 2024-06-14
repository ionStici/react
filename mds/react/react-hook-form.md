# React Hook Form

[React Hook Form](https://www.react-hook-form.com/) is a library for managing forms in React applications.

```
npm install react-hook-form
```

```jsx
import { useForm } from 'react-hook-form';

export default function App() {
  const defaultValues = { defaultValues: { name: 'John' } };
  const { register, handleSubmit, formState } = useForm(defaultValues);
  const { errors } = formState;

  const onSubmit = data => console.log(data);
  const onError = err => console.log(err);

  return (
    <form onSubmit={handleSubmit(onSubmit, onError)}>
      <input type="text" {...register('name', { required: 'Required' })} />
      {errors?.name?.message && <span>{errors.name.message}</span>}
    </form>
  );
}
```

- `useForm` : the main hook that provides methods and state for form management.
- `register` : function to connect input elements and their validation rules.
- `handleSubmit` : function to handle the form submission.
- `formState: { errors }` : an object that holds validation error messages.

## Validation

```jsx
<input
  type="number"
  {...register('age', {
    required: 'Required',
    min: { value: 18, message: '18+' },
    validate: value => Number.isInteger(+value) || 'Wrong format',
  })}
/>
```

```jsx
<input
  type="email"
  {...register('email', {
    required: 'Email is required',
    pattern: {
      value: /^[a-z0-9._%+-]+@[a-z0-9.-]+\.[a-z]{2,4}$/,
      message: 'Invalid email address',
    },
  })}
/>
```
