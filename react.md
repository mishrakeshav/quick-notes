 **React**
- Component Based
- Declarative
- Single Page Applications

- Components are the building blocks

1. React creates a virtual DOM tree when a component renders.
2. If state or props change, React diffs the new virtual DOM with the previous one.
3. Only the changed nodes are updated in the real DOM.

**Props** - readonly data passed from parent to child components  
**State** - Local data that can change and cause the UI to rerender  

**Lifecycle methods**
- Mounting
- Updating
- Unmounting
- `useEffect`: Handles side effects (like `componentDidMount`, `componentDidUpdate`, and `componentWillUnmount`).

**Example:**
```js
// Runs once (like componentDidMount)
useEffect(() => {
   console.log('Component mounted!');

   // Cleanup function (like componentWillUnmount)
   return () => console.log('Component will unmount!');
}, []);

// Runs every time count changes (like componentDidUpdate)
useEffect(() => {
   console.log('Count changed:', count);
}, [count]);
```
**useLayoutEffect**
- Runs synchronously immediately after all DOM mutations but before the browser has a chance to paint.

**useRef**
- Hook that returns a mutable ref object whose `.current` can hold any value.
- Does not cause rerender on change and retains the value on rerenders.
- Can be used for accessing DOM elements or storing mutable values, etc.

There are majorly 4 hooks that are used:
- `useState` - to manage state
- `useEffect` - to manage lifecycle and side effects of a component
- `useContext` - provides a way to access context values
- `useReducer` - A hook for managing complex state logic in functional components

### `createContext`, `useContext`
- Centralized state management
- Decoupling - child does not need to know where the state comes from
- Reactivity

**Custom hooks**
- It usually starts with the use phrase like `useForm`, `useFetch`, `useTimer`.
- Can use `useEffect`, contexts, etc.
- **Benefits** - Encapsulation, Lifecycle Management

### `useReducer` - Centralized state management
- Provides a more structured way to handle complex state updates.
- Dispatch an action to a reducer function that determines how the state should update.

### Lazy Loading and Code Splitting
```javascript
import React, { Suspense } from 'react';

const LazyComponent = React.lazy(() => import('./LazyComponent'));
```

### Notes on debounce and throttle 
```js 
function debounce(func, delay) {
  let timeoutId;

  return function (...args) {
    if(timeoutId){
      clearTimeout(timeoutId);
    }
    timeoutId = setTimeout(() => func(...args), delay);
  };
}

const DebounceExample = () => {
  const [searchTerm, setSearchTerm] = useState('');

  const handleChange = (event) => {
    setSearchTerm(event.target.value);
  };

  // Using useCallback to ensure the debounced function does not get recreated on every render
  const debouncedSearch = useCallback(debounce((query) => {
    console.log('Searching for:', query);
    // Here, you can call your API or perform the search operation
  }, 100), []);

  const handleInputChange = (event) => {
    handleChange(event);
    debouncedSearch(event.target.value);
  };

  return (
    <div>
      <input
        type="text"
        value={searchTerm}
        onChange={handleInputChange}
        placeholder="Search..."
      />
    </div>
  );
};```
