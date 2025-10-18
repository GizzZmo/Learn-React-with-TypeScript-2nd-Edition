# Chapter 4: Using React Hooks

This chapter explores React Hooks, which allow you to use state and other React features in function components.

## What You'll Learn

- Using the `useEffect` hook for side effects
- Managing state with `useState` and `useReducer`
- Referencing DOM elements with `useRef`
- Optimizing performance with `useMemo` and `useCallback`
- Understanding hook rules and best practices

## Prerequisites

Basic understanding of:
- React components (Chapter 1)
- TypeScript types (Chapter 2)
- React project setup (Chapter 3)

## Examples Overview

---

## Section 1: Using the Effect Hook

**Location:** `Section1-Using-the-effect-hook/`

Learn how to perform side effects in function components using the `useEffect` hook.

### What is useEffect?

The `useEffect` hook lets you perform side effects in function components:
- Data fetching
- Subscriptions
- DOM manipulation
- Timers
- Logging

### Running the Example

```bash
cd Section1-Using-the-effect-hook
npm install
npm start
```

### Key Concepts

**Basic Syntax:**
```typescript
useEffect(() => {
  // Side effect code here
  
  return () => {
    // Cleanup code (optional)
  };
}, [dependencies]);
```

**Example from this section:**
```typescript
import { useEffect } from 'react';
import { getPerson } from './getPerson';

export function PersonScore() {
  useEffect(() => {
    getPerson().then((person) => console.log(person));
  }, []);
  
  return null;
}
```

### Dependency Array Patterns

```typescript
// Runs once after mount (empty array)
useEffect(() => {
  console.log('Component mounted');
}, []);

// Runs on every render (no array)
useEffect(() => {
  console.log('Component rendered');
});

// Runs when dependencies change
useEffect(() => {
  console.log('Count changed:', count);
}, [count]);

// Multiple dependencies
useEffect(() => {
  console.log('Either changed:', name, age);
}, [name, age]);
```

### Cleanup Functions

```typescript
useEffect(() => {
  // Subscribe to something
  const subscription = subscribeToData();
  
  // Cleanup function
  return () => {
    subscription.unsubscribe();
  };
}, []);
```

### Common Use Cases

**Data Fetching:**
```typescript
useEffect(() => {
  async function fetchData() {
    const response = await fetch('/api/data');
    const data = await response.json();
    setData(data);
  }
  fetchData();
}, []);
```

**Event Listeners:**
```typescript
useEffect(() => {
  function handleScroll() {
    console.log('Scrolled!');
  }
  
  window.addEventListener('scroll', handleScroll);
  
  return () => {
    window.removeEventListener('scroll', handleScroll);
  };
}, []);
```

---

## Section 2: Using State Hooks

### 1. Using useState

**Location:** `Section2-Using-state-hooks/1-Using-useState/`

The `useState` hook is the most common way to add state to function components.

#### Running the Example

```bash
cd Section2-Using-state-hooks/1-Using-useState
npm install
npm start
```

#### Basic Usage

```typescript
import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>
        Increment
      </button>
    </div>
  );
}
```

#### TypeScript with useState

```typescript
// Type inference (recommended when possible)
const [name, setName] = useState('John');  // string

// Explicit typing
const [count, setCount] = useState<number>(0);

// Complex types
interface User {
  id: number;
  name: string;
}

const [user, setUser] = useState<User | null>(null);

// Array state
const [items, setItems] = useState<string[]>([]);
```

#### Updating State

```typescript
// Direct value
setCount(5);

// Function updater (recommended for updates based on previous state)
setCount(prevCount => prevCount + 1);

// Object state (don't mutate!)
const [user, setUser] = useState({ name: 'John', age: 30 });

// Wrong ❌
user.age = 31;
setUser(user);

// Correct ✓
setUser({ ...user, age: 31 });
setUser(prevUser => ({ ...prevUser, age: 31 }));
```

#### Multiple State Variables

```typescript
function Form() {
  const [name, setName] = useState('');
  const [email, setEmail] = useState('');
  const [age, setAge] = useState(0);
  
  // Use multiple useState calls for independent state
}
```

---

### 2. Using useReducer

**Location:** `Section2-Using-state-hooks/2-Using-useReducer/`

The `useReducer` hook is better for complex state logic with multiple sub-values or when state depends on previous state.

#### Running the Example

```bash
cd Section2-Using-state-hooks/2-Using-useReducer
npm install
npm start
```

#### Basic Usage

```typescript
import { useReducer } from 'react';

// 1. Define state type
interface State {
  count: number;
  step: number;
}

// 2. Define action types
type Action = 
  | { type: 'increment' }
  | { type: 'decrement' }
  | { type: 'setStep'; step: number };

// 3. Define reducer function
function reducer(state: State, action: Action): State {
  switch (action.type) {
    case 'increment':
      return { ...state, count: state.count + state.step };
    case 'decrement':
      return { ...state, count: state.count - state.step };
    case 'setStep':
      return { ...state, step: action.step };
    default:
      return state;
  }
}

// 4. Use in component
function Counter() {
  const [state, dispatch] = useReducer(reducer, { count: 0, step: 1 });
  
  return (
    <div>
      <p>Count: {state.count}</p>
      <button onClick={() => dispatch({ type: 'increment' })}>
        +
      </button>
      <button onClick={() => dispatch({ type: 'decrement' })}>
        -
      </button>
    </div>
  );
}
```

#### When to Use useReducer

✅ **Use useReducer when:**
- State logic is complex
- Multiple related state values
- Next state depends on previous state
- State updates follow patterns (like Redux)

✅ **Use useState when:**
- Simple state (single value)
- Independent state values
- State updates are straightforward

#### useReducer vs Redux

- `useReducer` is like Redux but for component-level state
- Redux is for global application state
- Both use the same reducer pattern

---

## Section 3: Using the Ref Hook

**Location:** `Section3-Using-the-ref-hook/`

The `useRef` hook lets you reference a value that persists across renders and doesn't cause re-renders when changed.

### Running the Example

```bash
cd Section3-Using-the-ref-hook
npm install
npm start
```

### Key Concepts

**Accessing DOM Elements:**
```typescript
import { useRef } from 'react';

function TextInput() {
  const inputRef = useRef<HTMLInputElement>(null);
  
  const focusInput = () => {
    inputRef.current?.focus();
  };
  
  return (
    <>
      <input ref={inputRef} type="text" />
      <button onClick={focusInput}>Focus Input</button>
    </>
  );
}
```

**Storing Mutable Values:**
```typescript
function Timer() {
  const intervalRef = useRef<number | null>(null);
  
  const startTimer = () => {
    intervalRef.current = window.setInterval(() => {
      console.log('Tick');
    }, 1000);
  };
  
  const stopTimer = () => {
    if (intervalRef.current) {
      clearInterval(intervalRef.current);
    }
  };
  
  return (
    <div>
      <button onClick={startTimer}>Start</button>
      <button onClick={stopTimer}>Stop</button>
    </div>
  );
}
```

### useRef vs useState

| Feature | useRef | useState |
|---------|--------|----------|
| Causes re-render | No | Yes |
| Mutable | Yes | No (use setter) |
| Persists across renders | Yes | Yes |
| Use for DOM access | Yes | No |

---

## Section 4: Using the Memo Hook

**Location:** `Section4-Using-the-memo-hook/`

The `useMemo` hook memoizes expensive calculations, recomputing only when dependencies change.

### Running the Example

```bash
cd Section4-Using-the-memo-hook
npm install
npm start
```

### Key Concepts

**Basic Usage:**
```typescript
import { useMemo } from 'react';

function ExpensiveComponent({ items }: { items: number[] }) {
  const expensiveValue = useMemo(() => {
    console.log('Computing expensive value...');
    return items.reduce((sum, item) => sum + item, 0);
  }, [items]);  // Only recompute when items change
  
  return <div>Total: {expensiveValue}</div>;
}
```

**TypeScript with useMemo:**
```typescript
// Type inference
const sum = useMemo(() => items.reduce((a, b) => a + b, 0), [items]);

// Explicit typing
const sum = useMemo<number>(() => {
  return items.reduce((a, b) => a + b, 0);
}, [items]);
```

### When to Use useMemo

✅ **Use useMemo when:**
- Expensive calculations
- Large data processing
- Filtering/sorting large lists
- Complex object creation

❌ **Don't use useMemo for:**
- Simple calculations
- Premature optimization
- Every calculation (adds overhead)

### Example: Filtering a Large List

```typescript
function ProductList({ products, searchTerm }: Props) {
  const filteredProducts = useMemo(() => {
    console.log('Filtering products...');
    return products.filter(p => 
      p.name.toLowerCase().includes(searchTerm.toLowerCase())
    );
  }, [products, searchTerm]);
  
  return (
    <ul>
      {filteredProducts.map(p => (
        <li key={p.id}>{p.name}</li>
      ))}
    </ul>
  );
}
```

---

## Section 5: Using the Callback Hook

**Location:** `Section5-Using-the-callback-hook/`

The `useCallback` hook memoizes callback functions, preventing unnecessary re-creations.

### Running the Example

```bash
cd Section5-Using-the-callback-hook
npm install
npm start
```

### Key Concepts

**Basic Usage:**
```typescript
import { useCallback } from 'react';

function Parent() {
  const [count, setCount] = useState(0);
  
  // Without useCallback: new function on every render
  const handleClick = () => {
    console.log('Clicked!');
  };
  
  // With useCallback: same function reference
  const handleClickMemo = useCallback(() => {
    console.log('Clicked!');
  }, []);  // Empty array: never recreated
  
  return <Child onClick={handleClickMemo} />;
}
```

**With Dependencies:**
```typescript
function TodoList({ todos }: { todos: Todo[] }) {
  const [filter, setFilter] = useState('all');
  
  const handleToggle = useCallback((id: number) => {
    // Update todo logic
  }, [todos]);  // Recreate when todos change
  
  return <div>...</div>;
}
```

### useCallback vs useMemo

```typescript
// useCallback returns a memoized function
const memoizedFn = useCallback(() => {
  doSomething();
}, [dependency]);

// useMemo returns a memoized value
const memoizedValue = useMemo(() => {
  return computeExpensiveValue();
}, [dependency]);

// These are equivalent:
useCallback(fn, deps) === useMemo(() => fn, deps)
```

### When to Use useCallback

✅ **Use useCallback when:**
- Passing callbacks to optimized child components (React.memo)
- Callbacks are dependencies of other hooks
- Preventing unnecessary re-renders

❌ **Don't use useCallback for:**
- Every callback (adds overhead)
- Simple event handlers
- Callbacks not passed to child components

### Example with React.memo

```typescript
import { memo, useCallback } from 'react';

// Memoized child component
const ChildButton = memo(({ onClick, text }: Props) => {
  console.log('ChildButton rendered');
  return <button onClick={onClick}>{text}</button>;
});

function Parent() {
  const [count, setCount] = useState(0);
  
  // Without useCallback: ChildButton re-renders on every Parent render
  // With useCallback: ChildButton only re-renders when needed
  const handleClick = useCallback(() => {
    console.log('Button clicked!');
  }, []);
  
  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <ChildButton onClick={handleClick} text="Click me" />
    </div>
  );
}
```

---

## Hook Rules

### Rules You Must Follow

1. **Only call hooks at the top level**
   - Don't call hooks inside loops, conditions, or nested functions
   
```typescript
// Wrong ❌
if (condition) {
  const [state, setState] = useState(0);
}

// Correct ✓
const [state, setState] = useState(0);
if (condition) {
  // Use state here
}
```

2. **Only call hooks from React functions**
   - Call hooks from function components
   - Call hooks from custom hooks
   
```typescript
// Wrong ❌
function regularFunction() {
  const [state, setState] = useState(0);
}

// Correct ✓
function MyComponent() {
  const [state, setState] = useState(0);
}
```

---

## Key Takeaways

By the end of this chapter, you should understand:

✅ How to use `useEffect` for side effects  
✅ Managing state with `useState` and `useReducer`  
✅ Accessing DOM elements with `useRef`  
✅ Optimizing with `useMemo` and `useCallback`  
✅ Hook rules and best practices  
✅ TypeScript typing for hooks  

---

## Common Patterns

### Custom Hook Pattern
```typescript
function useLocalStorage<T>(key: string, initialValue: T) {
  const [value, setValue] = useState<T>(() => {
    const item = localStorage.getItem(key);
    return item ? JSON.parse(item) : initialValue;
  });
  
  useEffect(() => {
    localStorage.setItem(key, JSON.stringify(value));
  }, [key, value]);
  
  return [value, setValue] as const;
}
```

### Data Fetching Pattern
```typescript
function useUser(userId: number) {
  const [user, setUser] = useState<User | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);
  
  useEffect(() => {
    async function fetchUser() {
      try {
        setLoading(true);
        const response = await fetch(`/api/users/${userId}`);
        const data = await response.json();
        setUser(data);
      } catch (err) {
        setError(err as Error);
      } finally {
        setLoading(false);
      }
    }
    
    fetchUser();
  }, [userId]);
  
  return { user, loading, error };
}
```

---

## Next Steps

- **Chapter 5**: Learn different approaches to styling
- **Chapter 6**: Add routing with React Router
- **Chapter 11**: Create custom hooks for reusable logic

---

## Resources

- **React Hooks Docs**: https://react.dev/reference/react
- **TypeScript + Hooks**: https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/hooks
- **Hook Rules**: https://react.dev/reference/rules/rules-of-hooks
- **ESLint Plugin**: https://www.npmjs.com/package/eslint-plugin-react-hooks
