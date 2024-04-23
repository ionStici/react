# react-datepicker library

```
npm i react-datepicker
```

[react-datepicker on npm](https://www.npmjs.com/package/react-datepicker)

```jsx
import DatePicker from "react-datepicker";
import "react-datepicker/dist/react-datepicker.css";

function Example() {
  const [startDate, setStartDate] = useState(new Date());

  return (
    <DatePicker selected={startDate} onChange={(date) => setStartDate(date)} />
  );
}
```
