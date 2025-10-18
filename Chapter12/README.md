# Chapter 12: Unit Testing with Jest and React Testing Library

This chapter teaches you how to write effective tests for React components using Jest and React Testing Library.

## What You'll Learn

- Setting up Jest and React Testing Library
- Testing component rendering
- Testing user interactions
- Testing async behavior
- Mocking dependencies
- Testing custom hooks
- Best practices for testing

## Examples Overview

This chapter has two versions of the same application:

---

## Start Project

**Location:** `start/`

The starting point - an application without tests.

### Running the Example

```bash
cd start
npm install
npm start
```

Explore the application to understand what needs to be tested.

---

## Finish Project

**Location:** `finish/`

The complete application with comprehensive tests.

### Running the Tests

```bash
cd finish
npm install
npm test
```

### Running Tests in Watch Mode

```bash
npm test -- --watch
```

### Running Coverage Report

```bash
npm test -- --coverage
```

---

## Key Concepts

### Testing Philosophy

React Testing Library encourages testing your components the way users interact with them:

✅ **Test behavior, not implementation**  
✅ **Query by accessible roles and text**  
✅ **Test user interactions**  
✅ **Avoid testing internal state**  

### Basic Component Test

```typescript
import { render, screen } from '@testing-library/react';
import Button from './Button';

describe('Button', () => {
  it('renders button with text', () => {
    render(<Button>Click me</Button>);
    
    const button = screen.getByRole('button', { name: /click me/i });
    expect(button).toBeInTheDocument();
  });
  
  it('calls onClick when clicked', () => {
    const handleClick = jest.fn();
    render(<Button onClick={handleClick}>Click me</Button>);
    
    const button = screen.getByRole('button', { name: /click me/i });
    button.click();
    
    expect(handleClick).toHaveBeenCalledTimes(1);
  });
});
```

### Testing User Interactions

```typescript
import { render, screen } from '@testing-library/react';
import userEvent from '@testing-library/user-event';
import Form from './Form';

describe('Form', () => {
  it('submits form with user input', async () => {
    const handleSubmit = jest.fn();
    const user = userEvent.setup();
    
    render(<Form onSubmit={handleSubmit} />);
    
    // Fill in form
    await user.type(screen.getByLabelText(/name/i), 'John Doe');
    await user.type(screen.getByLabelText(/email/i), 'john@example.com');
    
    // Submit form
    await user.click(screen.getByRole('button', { name: /submit/i }));
    
    // Verify submission
    expect(handleSubmit).toHaveBeenCalledWith({
      name: 'John Doe',
      email: 'john@example.com'
    });
  });
});
```

### Testing Async Behavior

```typescript
import { render, screen, waitFor } from '@testing-library/react';
import PostsList from './PostsList';

describe('PostsList', () => {
  it('loads and displays posts', async () => {
    // Mock API
    global.fetch = jest.fn(() =>
      Promise.resolve({
        ok: true,
        json: async () => [
          { id: 1, title: 'Post 1' },
          { id: 2, title: 'Post 2' }
        ]
      })
    ) as jest.Mock;
    
    render(<PostsList />);
    
    // Check loading state
    expect(screen.getByText(/loading/i)).toBeInTheDocument();
    
    // Wait for posts to load
    await waitFor(() => {
      expect(screen.getByText('Post 1')).toBeInTheDocument();
    });
    
    expect(screen.getByText('Post 2')).toBeInTheDocument();
  });
  
  it('handles error state', async () => {
    global.fetch = jest.fn(() =>
      Promise.reject(new Error('Failed to fetch'))
    ) as jest.Mock;
    
    render(<PostsList />);
    
    await waitFor(() => {
      expect(screen.getByText(/error/i)).toBeInTheDocument();
    });
  });
});
```

### Testing Components with Context

```typescript
import { render, screen } from '@testing-library/react';
import { UserProvider } from './context/UserContext';
import Profile from './Profile';

function renderWithUser(ui: React.ReactElement, user = null) {
  return render(
    <UserProvider initialUser={user}>
      {ui}
    </UserProvider>
  );
}

describe('Profile', () => {
  it('shows login prompt when not authenticated', () => {
    renderWithUser(<Profile />, null);
    expect(screen.getByText(/please log in/i)).toBeInTheDocument();
  });
  
  it('shows user info when authenticated', () => {
    const user = { name: 'John Doe', email: 'john@example.com' };
    renderWithUser(<Profile />, user);
    
    expect(screen.getByText('John Doe')).toBeInTheDocument();
    expect(screen.getByText('john@example.com')).toBeInTheDocument();
  });
});
```

### Testing Custom Hooks

```typescript
import { renderHook, act } from '@testing-library/react';
import useCounter from './useCounter';

describe('useCounter', () => {
  it('initializes with default value', () => {
    const { result } = renderHook(() => useCounter());
    expect(result.current.count).toBe(0);
  });
  
  it('initializes with custom value', () => {
    const { result } = renderHook(() => useCounter(10));
    expect(result.current.count).toBe(10);
  });
  
  it('increments count', () => {
    const { result } = renderHook(() => useCounter());
    
    act(() => {
      result.current.increment();
    });
    
    expect(result.current.count).toBe(1);
  });
  
  it('decrements count', () => {
    const { result } = renderHook(() => useCounter(5));
    
    act(() => {
      result.current.decrement();
    });
    
    expect(result.current.count).toBe(4);
  });
});
```

### Mocking Modules

```typescript
// Mock entire module
jest.mock('./api', () => ({
  fetchPosts: jest.fn()
}));

import { fetchPosts } from './api';

describe('PostsList', () => {
  it('fetches posts on mount', async () => {
    const mockPosts = [{ id: 1, title: 'Post 1' }];
    (fetchPosts as jest.Mock).mockResolvedValue(mockPosts);
    
    render(<PostsList />);
    
    await waitFor(() => {
      expect(fetchPosts).toHaveBeenCalledTimes(1);
    });
  });
});
```

### Mocking React Router

```typescript
import { render, screen } from '@testing-library/react';
import { MemoryRouter, Routes, Route } from 'react-router-dom';
import PostPage from './PostPage';

function renderWithRouter(ui: React.ReactElement, initialRoute = '/') {
  return render(
    <MemoryRouter initialEntries={[initialRoute]}>
      <Routes>
        <Route path="/posts/:id" element={ui} />
      </Routes>
    </MemoryRouter>
  );
}

describe('PostPage', () => {
  it('renders post with id from URL', () => {
    renderWithRouter(<PostPage />, '/posts/123');
    // Test component with id=123
  });
});
```

---

## Query Methods

### Priority Order (Best Practices)

1. **getByRole** - Queries by ARIA role (best for accessibility)
   ```typescript
   screen.getByRole('button', { name: /submit/i })
   screen.getByRole('textbox', { name: /email/i })
   ```

2. **getByLabelText** - Forms with labels
   ```typescript
   screen.getByLabelText(/email/i)
   ```

3. **getByPlaceholderText** - Forms with placeholders
   ```typescript
   screen.getByPlaceholderText(/enter email/i)
   ```

4. **getByText** - Non-interactive content
   ```typescript
   screen.getByText(/hello world/i)
   ```

5. **getByTestId** - Last resort
   ```typescript
   screen.getByTestId('custom-element')
   ```

### Query Variants

- **getBy...** - Returns element or throws error
- **queryBy...** - Returns element or null (use for asserting non-existence)
- **findBy...** - Returns promise (use for async elements)

```typescript
// Throw error if not found
const button = screen.getByRole('button');

// Return null if not found
const button = screen.queryByRole('button');
expect(button).not.toBeInTheDocument();

// Wait for element to appear
const button = await screen.findByRole('button');
```

---

## Common Assertions

```typescript
// Element exists
expect(element).toBeInTheDocument();

// Element is visible
expect(element).toBeVisible();

// Element has text
expect(element).toHaveTextContent('Hello');

// Element is disabled
expect(element).toBeDisabled();

// Element has class
expect(element).toHaveClass('active');

// Element has attribute
expect(element).toHaveAttribute('href', '/home');

// Function was called
expect(mockFn).toHaveBeenCalled();
expect(mockFn).toHaveBeenCalledTimes(2);
expect(mockFn).toHaveBeenCalledWith('arg1', 'arg2');
```

---

## Test Organization

### Describe Blocks

```typescript
describe('Button', () => {
  describe('rendering', () => {
    it('renders with text', () => {/* ... */});
    it('renders with icon', () => {/* ... */});
  });
  
  describe('interactions', () => {
    it('calls onClick when clicked', () => {/* ... */});
    it('is disabled when loading', () => {/* ... */});
  });
});
```

### Setup and Teardown

```typescript
describe('Form', () => {
  let mockOnSubmit: jest.Mock;
  
  beforeEach(() => {
    mockOnSubmit = jest.fn();
  });
  
  afterEach(() => {
    jest.clearAllMocks();
  });
  
  it('test 1', () => {/* uses mockOnSubmit */});
  it('test 2', () => {/* uses mockOnSubmit */});
});
```

---

## Best Practices

### ✅ Do

- Test user-facing behavior
- Use accessible queries (role, label)
- Test error states
- Test loading states
- Mock external dependencies
- Write readable test descriptions
- Test one thing per test

### ❌ Don't

- Test implementation details
- Test internal state directly
- Use `.instance()` or `.state()`
- Query by class names or IDs unless necessary
- Create brittle selectors
- Write tests that depend on each other

---

## TypeScript with Tests

```typescript
import { render, screen, RenderResult } from '@testing-library/react';

interface RenderOptions {
  user?: User;
  theme?: Theme;
}

function customRender(
  ui: React.ReactElement,
  options?: RenderOptions
): RenderResult {
  return render(
    <Providers {...options}>
      {ui}
    </Providers>
  );
}

// Type-safe mock
const mockFn = jest.fn<ReturnType, [ParamType]>();
```

---

## Coverage Goals

Aim for:
- **80%+ statements** covered
- **80%+ branches** covered
- **80%+ functions** covered
- **80%+ lines** covered

But remember: **100% coverage ≠ 100% tested**

Focus on testing critical paths and user flows.

---

## Key Takeaways

✅ Test behavior, not implementation  
✅ Use accessible queries  
✅ Test user interactions  
✅ Mock external dependencies  
✅ Test async behavior with waitFor  
✅ Organize tests logically  
✅ Write readable test descriptions  
✅ Aim for good coverage, but focus on quality  

---

## Next Steps

- Set up continuous integration (CI)
- Add e2e tests with Cypress or Playwright
- Implement visual regression testing
- Add performance testing

---

## Resources

- **React Testing Library**: https://testing-library.com/react
- **Jest**: https://jestjs.io/
- **Testing Library Queries**: https://testing-library.com/docs/queries/about
- **Common Mistakes**: https://kentcdodds.com/blog/common-mistakes-with-react-testing-library
- **Testing Playground**: https://testing-playground.com/
