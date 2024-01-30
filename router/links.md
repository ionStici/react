# Linking to Routes

```js
import { Link, NavLink } from "react-router-dom";

<Link to="/about">About</Link>

<NavLink to="/about">About</NavLink>
```

- `Link` and `NavLink` components work like anchor tags.
- The `to` prop indicates the location to redirect the user to.

Both links will generate anchor tags displaying the given text and will take the user to the given path indicated within `to` prop when clicked.

The default behavior of an anchor tag (refreshing when the page loads) will be disabled.

If we provide a `/` forward slash, the path will be treated as an absolute path. React Router will assume this path is navigating from the root directory.

Unlike the `Link` component, `NavLink` will automatically get an `'active'` class applied to it when the path from the `to` prop matches.

```js
<NavLink
  to="about"
  className={({ isActive }) => (isActive ? "activeLink" : "inactiveLink")}
>
  About
</NavLink>
```

We can pass a function to `className` or `style` to further customize the styling of an active or inactive `NavLink`:
