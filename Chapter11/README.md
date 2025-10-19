# Chapter 11: Reusable Components

This chapter demonstrates patterns and techniques for building flexible, reusable React components with TypeScript.

## What You'll Learn

- Using generic props for type flexibility
- Props spreading patterns
- Render props pattern
- Creating custom hooks
- Controlled vs uncontrolled components
- Building component libraries

## Examples Overview

All examples are complete React applications demonstrating advanced component patterns.

---

## Example 1: Using Generic Props

**Location:** `Using-generic-props/`

Create components that work with different data types using TypeScript generics.

### Running the Example

```bash
cd Using-generic-props
npm install
npm start
```

### Key Concepts

```typescript
interface ListProps<T> {
  items: T[];
  renderItem: (item: T) => React.ReactNode;
  keyExtractor: (item: T) => string | number;
}

function List<T>({ items, renderItem, keyExtractor }: ListProps<T>) {
  return (
    <ul>
      {items.map(item => (
        <li key={keyExtractor(item)}>
          {renderItem(item)}
        </li>
      ))}
    </ul>
  );
}

// Usage with different types
interface User {
  id: number;
  name: string;
}

interface Product {
  id: string;
  title: string;
  price: number;
}

function App() {
  const users: User[] = [/* ... */];
  const products: Product[] = [/* ... */];
  
  return (
    <>
      <List
        items={users}
        renderItem={user => <span>{user.name}</span>}
        keyExtractor={user => user.id}
      />
      
      <List
        items={products}
        renderItem={product => (
          <span>{product.title} - ${product.price}</span>
        )}
        keyExtractor={product => product.id}
      />
    </>
  );
}
```

### Generic Select Component

```typescript
interface SelectProps<T> {
  options: T[];
  value: T;
  onChange: (value: T) => void;
  getLabel: (option: T) => string;
  getValue: (option: T) => string;
}

function Select<T>({ options, value, onChange, getLabel, getValue }: SelectProps<T>) {
  return (
    <select
      value={getValue(value)}
      onChange={e => {
        const selected = options.find(opt => getValue(opt) === e.target.value);
        if (selected) onChange(selected);
      }}
    >
      {options.map(option => (
        <option key={getValue(option)} value={getValue(option)}>
          {getLabel(option)}
        </option>
      ))}
    </select>
  );
}
```

---

## Example 2: Using Props Spreading

**Location:** `Using-props-spreading/`

Forward props to child elements efficiently.

### Running the Example

```bash
cd Using-props-spreading
npm install
npm start
```

### Key Concepts

```typescript
import { ButtonHTMLAttributes } from 'react';

interface ButtonProps extends ButtonHTMLAttributes<HTMLButtonElement> {
  variant?: 'primary' | 'secondary';
  loading?: boolean;
}

function Button({ variant = 'primary', loading, children, ...rest }: ButtonProps) {
  const className = `btn btn-${variant} ${rest.className || ''}`;
  
  return (
    <button
      {...rest}
      className={className}
      disabled={loading || rest.disabled}
    >
      {loading ? 'Loading...' : children}
    </button>
  );
}

// Usage - all button props are supported!
<Button onClick={handleClick} type="submit" variant="primary">
  Submit
</Button>
```

### Input Component with Ref Forwarding

```typescript
import { forwardRef, InputHTMLAttributes } from 'react';

interface InputProps extends InputHTMLAttributes<HTMLInputElement> {
  label?: string;
  error?: string;
}

const Input = forwardRef<HTMLInputElement, InputProps>(
  ({ label, error, ...rest }, ref) => {
    return (
      <div>
        {label && <label>{label}</label>}
        <input ref={ref} {...rest} />
        {error && <span className="error">{error}</span>}
      </div>
    );
  }
);

// Usage with ref
function Form() {
  const inputRef = useRef<HTMLInputElement>(null);
  
  return (
    <Input
      ref={inputRef}
      label="Email"
      type="email"
      placeholder="Enter email"
      error={errors.email}
    />
  );
}
```

---

## Example 3: Using Render Props

**Location:** `Using-render-props/`

Share code between components using props that are functions.

### Running the Example

```bash
cd Using-render-props
npm install
npm start
```

### Key Concepts

```typescript
interface MousePosition {
  x: number;
  y: number;
}

interface MouseTrackerProps {
  render: (position: MousePosition) => React.ReactNode;
}

function MouseTracker({ render }: MouseTrackerProps) {
  const [position, setPosition] = useState<MousePosition>({ x: 0, y: 0 });
  
  useEffect(() => {
    const handleMove = (e: MouseEvent) => {
      setPosition({ x: e.clientX, y: e.clientY });
    };
    
    window.addEventListener('mousemove', handleMove);
    return () => window.removeEventListener('mousemove', handleMove);
  }, []);
  
  return <>{render(position)}</>;
}

// Usage
function App() {
  return (
    <MouseTracker
      render={({ x, y }) => (
        <div>
          Mouse position: {x}, {y}
        </div>
      )}
    />
  );
}
```

### Children as Function

```typescript
interface ToggleProps {
  children: (isOpen: boolean, toggle: () => void) => React.ReactNode;
}

function Toggle({ children }: ToggleProps) {
  const [isOpen, setIsOpen] = useState(false);
  const toggle = () => setIsOpen(!isOpen);
  
  return <>{children(isOpen, toggle)}</>;
}

// Usage
<Toggle>
  {(isOpen, toggle) => (
    <>
      <button onClick={toggle}>
        {isOpen ? 'Close' : 'Open'}
      </button>
      {isOpen && <div>Content</div>}
    </>
  )}
</Toggle>
```

---

## Example 4: Creating Custom Hooks

**Location:** `Creating-custom-hooks/`

Extract reusable logic into custom hooks.

### Running the Example

```bash
cd Creating-custom-hooks
npm install
npm start
```

### Key Concepts

**useLocalStorage Hook:**
```typescript
function useLocalStorage<T>(key: string, initialValue: T) {
  const [value, setValue] = useState<T>(() => {
    try {
      const item = window.localStorage.getItem(key);
      return item ? JSON.parse(item) : initialValue;
    } catch (error) {
      return initialValue;
    }
  });
  
  const setStoredValue = (newValue: T | ((val: T) => T)) => {
    try {
      const valueToStore = newValue instanceof Function ? newValue(value) : newValue;
      setValue(valueToStore);
      window.localStorage.setItem(key, JSON.stringify(valueToStore));
    } catch (error) {
      console.error(error);
    }
  };
  
  return [value, setStoredValue] as const;
}

// Usage
function App() {
  const [name, setName] = useLocalStorage('name', 'Guest');
  
  return (
    <input
      value={name}
      onChange={e => setName(e.target.value)}
    />
  );
}
```

**useFetch Hook:**
```typescript
interface UseFetchResult<T> {
  data: T | null;
  loading: boolean;
  error: Error | null;
  refetch: () => void;
}

function useFetch<T>(url: string): UseFetchResult<T> {
  const [data, setData] = useState<T | null>(null);
  const [loading, setLoading] = useState(true);
  const [error, setError] = useState<Error | null>(null);
  
  const fetchData = useCallback(async () => {
    try {
      setLoading(true);
      const response = await fetch(url);
      if (!response.ok) throw new Error('Fetch failed');
      const json = await response.json();
      setData(json);
    } catch (err) {
      setError(err as Error);
    } finally {
      setLoading(false);
    }
  }, [url]);
  
  useEffect(() => {
    fetchData();
  }, [fetchData]);
  
  return { data, loading, error, refetch: fetchData };
}

// Usage
function Posts() {
  const { data, loading, error, refetch } = useFetch<Post[]>('/api/posts');
  
  if (loading) return <div>Loading...</div>;
  if (error) return <div>Error: {error.message}</div>;
  
  return (
    <>
      <button onClick={refetch}>Refresh</button>
      <ul>
        {data?.map(post => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </>
  );
}
```

**useToggle Hook:**
```typescript
function useToggle(initialValue = false): [boolean, () => void] {
  const [value, setValue] = useState(initialValue);
  const toggle = useCallback(() => setValue(v => !v), []);
  return [value, toggle];
}

// Usage
function Dropdown() {
  const [isOpen, toggleOpen] = useToggle(false);
  
  return (
    <>
      <button onClick={toggleOpen}>Toggle</button>
      {isOpen && <div>Dropdown content</div>}
    </>
  );
}
```

---

## Example 5: Adding Checked Functionality

**Location:** `Adding-checked-functionality/`

Building checkbox components with proper state management.

### Running the Example

```bash
cd Adding-checked-functionality
npm install
npm start
```

### Key Concepts

```typescript
interface CheckboxProps {
  checked: boolean;
  onChange: (checked: boolean) => void;
  label: string;
}

function Checkbox({ checked, onChange, label }: CheckboxProps) {
  return (
    <label>
      <input
        type="checkbox"
        checked={checked}
        onChange={e => onChange(e.target.checked)}
      />
      {label}
    </label>
  );
}

// Usage
function App() {
  const [agreed, setAgreed] = useState(false);
  
  return (
    <Checkbox
      checked={agreed}
      onChange={setAgreed}
      label="I agree to terms"
    />
  );
}
```

---

## Example 6: Allowing Internal State to be Controlled

**Location:** `Allowing-internal-state-to-be-controlled/`

Create components that can be either controlled or uncontrolled.

### Running the Example

```bash
cd Allowing-internal-state-to-be-controlled
npm install
npm start
```

### Key Concepts

```typescript
interface InputProps {
  value?: string;
  defaultValue?: string;
  onChange?: (value: string) => void;
}

function Input({ value: controlledValue, defaultValue, onChange }: InputProps) {
  const [internalValue, setInternalValue] = useState(defaultValue ?? '');
  
  // If value prop is provided, component is controlled
  const isControlled = controlledValue !== undefined;
  const value = isControlled ? controlledValue : internalValue;
  
  const handleChange = (newValue: string) => {
    if (!isControlled) {
      setInternalValue(newValue);
    }
    onChange?.(newValue);
  };
  
  return (
    <input
      value={value}
      onChange={e => handleChange(e.target.value)}
    />
  );
}

// Controlled usage
function ControlledExample() {
  const [value, setValue] = useState('');
  return <Input value={value} onChange={setValue} />;
}

// Uncontrolled usage
function UncontrolledExample() {
  return <Input defaultValue="Initial" onChange={console.log} />;
}
```

---

## Key Patterns Summary

### When to Use Each Pattern

| Pattern | Use Case |
|---------|----------|
| Generic Props | Reusable components for different data types |
| Props Spreading | Extending native HTML elements |
| Render Props | Sharing stateful logic (pre-hooks era) |
| Custom Hooks | Modern way to share logic (preferred) |
| Controlled/Uncontrolled | Flexible form components |

---

## Key Takeaways

✅ Generic props enable type-safe reusability  
✅ Props spreading extends HTML elements  
✅ Custom hooks are the modern way to share logic  
✅ Render props still useful for specific cases  
✅ Controlled/uncontrolled pattern offers flexibility  
✅ TypeScript enhances all these patterns  

---

## Next Steps

- **Chapter 12**: Test your reusable components
- Create your own component library
- Publish components to npm

---

## Resources

- **React Patterns**: https://react.dev/learn/reusing-logic-with-custom-hooks
- **TypeScript Generics**: https://www.typescriptlang.org/docs/handbook/2/generics.html
- **Component Libraries**: https://github.com/storybookjs/storybook
