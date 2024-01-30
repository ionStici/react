# Draft

## Context API

**Context API:** allows components everywhere in the tree to read state that a context shares; system to pass data throughout the app; it allows us to transmit global state to the entire app.

_Context API parts:_

1. **Provider:** gives all child components access to `value`
2. **Value:** the data that we want to make available (usually state and functions). The data that we want to transmit through the provider, so we pass it into the provider
3. **Consumers:** all components that read the provided context `value`

Whenever the context `value` is updated, _all consumers re-render_.

<br>

## Creating, Providing, Consuming a Context

```jsx
import { createContext, useContext } from "react";

// 1. Create a Context
const PostsContext = createContext(null);

function App() {
  const [posts, setPosts] = useState([]);
  const handleAddPost = (post) => setPosts((posts) => [...posts, post]);
  const handleClearPosts = () => setPosts([]);

  return (
    // 2. Provide Value to Child Components
    <PostsContext.Provider
      value={{
        posts,
        onAddPost: handleAddPost,
        onClearPosts: handleClearPosts,
      }}
    >
      <Components />
    </PostsContext.Provider>
  );
}

function Components() {
  // 3. Consuming Context Value
  const context = useContext(PostsContext);
  const { posts } = useContext(PostsContext);
}
```
