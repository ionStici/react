# Forms in React

- **Controlled Inputs:** the input's value is controlled by the component's state.

  Changes to the input update the state, and updates to the state reflect on the input.

- **Uncontrolled Inputs:** the input's state is managed by the DOM.

  References (refs) to the DOM elements are used to access input values.

```jsx
import { useState, useRef } from 'react';

function ControlledInput() {
  const [input, setInput] = useState('');
  const inputRef = useRef(null);

  const handleChange = ({ target }) => setInput(target.value);

  const handleSubmit = event => {
    event.preventDefault();
    console.log(input, inputRef.current.value);
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* Controlled Input: */}
      <input type="text" value={input} onChange={handleChange} />
      {/* Uncontrolled Input: */}
      <input type="text" ref={inputRef} />
    </form>
  );
}
```
