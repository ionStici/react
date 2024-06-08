# React Hook Form

```
npm install react-hook-form
```

[React Hook Form](https://www.react-hook-form.com/) is a library for managing forms in React applications.

```jsx
import { useForm } from 'react-hook-form';

function App() {
  const { register, handleSubmit, reset, getValues, formState } = useForm({
    defaultValues: {
      name: 'John',
    },
  });

  const { errors } = formState;

  const onSubmit = data => console.log(data);
  const onError = err => console.log(err);

  return (
    <form onSubmit={handleSubmit(onSubmit, onError)}>
      <div>
        <input type="text" {...register('name', { required: 'Required' })} />
        {errors?.name?.message && <span>{errors.name.message}</span>}
      </div>

      <div>
        <input
          type="number"
          {...register('age', {
            required: 'Required',
            min: { value: 18, message: '18+' },
            validate: value => Number.isInteger(+value) || 'Wrong format',
          })}
        />
        {errors?.age?.message && <span>{errors.age.message}</span>}
      </div>

      <div>
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
        {errors?.email?.message && <span>{errors.email.message}</span>}
      </div>
    </form>
  );
}
```

## How it works

### register

The `register` function connects input fields to the form state:

```jsx
<input {...register('name')} />
```

### handleSubmit

The `handleSubmit` function handles form submissions. The function passed to `handleSubmit` will receive the form data.

```jsx
<form onSubmit={handleSubmit(onSubmit, onError)}>
```

Optionally we can pass a second function that will handle errors.

### Validation

We can add validation rules directly within the `register` function.

```jsx
return (
  <div>
    <input type="text" {...register('name', { required: 'Required' })} />;
    {errors?.name?.message && <span>{errors.name.message}</span>}
  </div>
);
```

### reset

The `reset` function will reset the form field values.

```jsx
const onSubmit = data => reset();
```
