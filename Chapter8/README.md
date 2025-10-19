# Chapter 8: State Management

This chapter explores different approaches to managing application state in React, from simple prop drilling to advanced state management libraries.

## What You'll Learn

- Understanding prop drilling and its problems
- Using React Context API for global state
- Implementing Redux with Redux Toolkit
- When to use each approach
- TypeScript with state management

## Examples Overview

Each example demonstrates a different state management approach in a complete application.

---

## Example 1: Prop Drilling

**Location:** `prop-drilling/`

Demonstrates passing props through multiple component levels.

### Running the Example

```bash
cd prop-drilling
npm install
npm start
```

### The Problem

```typescript
// App.tsx - Top level
function App() {
  const [user, setUser] = useState<User | null>(null);
  
  return <Layout user={user} setUser={setUser} />;
}

// Layout.tsx - Middle level (doesn't use props)
function Layout({ user, setUser }: Props) {
  return (
    <div>
      <Header user={user} setUser={setUser} />
      <Sidebar user={user} />
    </div>
  );
}

// Header.tsx - Finally uses the props
function Header({ user, setUser }: Props) {
  return <UserProfile user={user} onLogout={() => setUser(null)} />;
}
```

### Issues with Prop Drilling

❌ Props passed through components that don't need them  
❌ Refactoring is difficult  
❌ Component reusability suffers  
❌ Code becomes verbose and hard to maintain  

### When It's OK

✅ 1-2 levels deep  
✅ Small applications  
✅ Props are actually used by intermediate components  

---

## Example 2: Context API

**Location:** `context/`

Using React's built-in Context API for global state management.

### Running the Example

```bash
cd context
npm install
npm start
```

### Key Concepts

**1. Create Context:**
```typescript
import { createContext, useContext, useState, ReactNode } from 'react';

interface User {
  id: string;
  name: string;
  email: string;
}

interface UserContextType {
  user: User | null;
  setUser: (user: User | null) => void;
  login: (email: string, password: string) => Promise<void>;
  logout: () => void;
}

const UserContext = createContext<UserContextType | undefined>(undefined);
```

**2. Create Provider:**
```typescript
interface UserProviderProps {
  children: ReactNode;
}

export function UserProvider({ children }: UserProviderProps) {
  const [user, setUser] = useState<User | null>(null);
  
  const login = async (email: string, password: string) => {
    // Login logic
    const userData = await loginApi(email, password);
    setUser(userData);
  };
  
  const logout = () => {
    setUser(null);
  };
  
  const value = {
    user,
    setUser,
    login,
    logout
  };
  
  return (
    <UserContext.Provider value={value}>
      {children}
    </UserContext.Provider>
  );
}
```

**3. Create Custom Hook:**
```typescript
export function useUser() {
  const context = useContext(UserContext);
  if (context === undefined) {
    throw new Error('useUser must be used within UserProvider');
  }
  return context;
}
```

**4. Wrap App:**
```typescript
// index.tsx or App.tsx
import { UserProvider } from './context/UserContext';

function App() {
  return (
    <UserProvider>
      <Header />
      <MainContent />
    </UserProvider>
  );
}
```

**5. Use in Components:**
```typescript
function Header() {
  const { user, logout } = useUser();
  
  return (
    <header>
      {user ? (
        <>
          <span>Welcome, {user.name}</span>
          <button onClick={logout}>Logout</button>
        </>
      ) : (
        <Link to="/login">Login</Link>
      )}
    </header>
  );
}
```

### Multiple Contexts

```typescript
function App() {
  return (
    <UserProvider>
      <ThemeProvider>
        <NotificationProvider>
          <Router />
        </NotificationProvider>
      </ThemeProvider>
    </UserProvider>
  );
}
```

### Context with Reducer

```typescript
interface State {
  user: User | null;
  loading: boolean;
  error: string | null;
}

type Action =
  | { type: 'LOGIN_START' }
  | { type: 'LOGIN_SUCCESS'; payload: User }
  | { type: 'LOGIN_ERROR'; payload: string }
  | { type: 'LOGOUT' };

function userReducer(state: State, action: Action): State {
  switch (action.type) {
    case 'LOGIN_START':
      return { ...state, loading: true, error: null };
    case 'LOGIN_SUCCESS':
      return { ...state, loading: false, user: action.payload };
    case 'LOGIN_ERROR':
      return { ...state, loading: false, error: action.payload };
    case 'LOGOUT':
      return { ...state, user: null };
    default:
      return state;
  }
}

function UserProvider({ children }: Props) {
  const [state, dispatch] = useReducer(userReducer, {
    user: null,
    loading: false,
    error: null
  });
  
  // ...
}
```

### Advantages

✅ Built into React (no dependencies)  
✅ Simple to understand  
✅ Good for small-medium apps  
✅ Avoids prop drilling  

### Disadvantages

❌ Can cause unnecessary re-renders  
❌ No built-in debugging tools  
❌ Not ideal for frequently changing data  
❌ Context hell with many contexts  

---

## Example 3: Redux with Redux Toolkit

**Location:** `redux/`

Using Redux Toolkit for predictable state management with excellent dev tools.

### Running the Example

```bash
cd redux
npm install
npm start
```

### Key Concepts

**1. Install Dependencies:**
```bash
npm install @reduxjs/toolkit react-redux
npm install --save-dev @types/react-redux
```

**2. Create Slice:**
```typescript
// features/user/userSlice.ts
import { createSlice, PayloadAction } from '@reduxjs/toolkit';

interface User {
  id: string;
  name: string;
  email: string;
}

interface UserState {
  user: User | null;
  loading: boolean;
  error: string | null;
}

const initialState: UserState = {
  user: null,
  loading: false,
  error: null
};

const userSlice = createSlice({
  name: 'user',
  initialState,
  reducers: {
    setUser: (state, action: PayloadAction<User>) => {
      state.user = action.payload;
    },
    clearUser: (state) => {
      state.user = null;
    },
    setLoading: (state, action: PayloadAction<boolean>) => {
      state.loading = action.payload;
    },
    setError: (state, action: PayloadAction<string>) => {
      state.error = action.payload;
    }
  }
});

export const { setUser, clearUser, setLoading, setError } = userSlice.actions;
export default userSlice.reducer;
```

**3. Configure Store:**
```typescript
// app/store.ts
import { configureStore } from '@reduxjs/toolkit';
import userReducer from '../features/user/userSlice';

export const store = configureStore({
  reducer: {
    user: userReducer
  }
});

export type RootState = ReturnType<typeof store.getState>;
export type AppDispatch = typeof store.dispatch;
```

**4. Create Typed Hooks:**
```typescript
// app/hooks.ts
import { TypedUseSelectorHook, useDispatch, useSelector } from 'react-redux';
import type { RootState, AppDispatch } from './store';

export const useAppDispatch = () => useDispatch<AppDispatch>();
export const useAppSelector: TypedUseSelectorHook<RootState> = useSelector;
```

**5. Provide Store:**
```typescript
// index.tsx
import { Provider } from 'react-redux';
import { store } from './app/store';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <Provider store={store}>
    <App />
  </Provider>
);
```

**6. Use in Components:**
```typescript
import { useAppSelector, useAppDispatch } from './app/hooks';
import { setUser, clearUser } from './features/user/userSlice';

function Header() {
  const user = useAppSelector((state) => state.user.user);
  const dispatch = useAppDispatch();
  
  const handleLogout = () => {
    dispatch(clearUser());
  };
  
  return (
    <header>
      {user && (
        <>
          <span>{user.name}</span>
          <button onClick={handleLogout}>Logout</button>
        </>
      )}
    </header>
  );
}
```

### Async Actions with Thunks

```typescript
import { createAsyncThunk } from '@reduxjs/toolkit';

export const loginUser = createAsyncThunk(
  'user/login',
  async ({ email, password }: LoginCredentials, { rejectWithValue }) => {
    try {
      const response = await fetch('/api/login', {
        method: 'POST',
        body: JSON.stringify({ email, password })
      });
      
      if (!response.ok) throw new Error('Login failed');
      
      return await response.json();
    } catch (error) {
      return rejectWithValue(error.message);
    }
  }
);

// In slice
extraReducers: (builder) => {
  builder
    .addCase(loginUser.pending, (state) => {
      state.loading = true;
    })
    .addCase(loginUser.fulfilled, (state, action) => {
      state.loading = false;
      state.user = action.payload;
    })
    .addCase(loginUser.rejected, (state, action) => {
      state.loading = false;
      state.error = action.payload as string;
    });
}
```

### Redux DevTools

Redux comes with powerful debugging tools:
- Time-travel debugging
- Action history
- State inspection
- Action replay

Install Redux DevTools browser extension for the best experience.

### Advantages

✅ Predictable state management  
✅ Excellent debugging tools  
✅ Great for large applications  
✅ Middleware support  
✅ Time-travel debugging  
✅ Strong TypeScript support  

### Disadvantages

❌ More boilerplate than Context  
❌ Learning curve  
❌ Overkill for small apps  
❌ Additional dependency  

---

## Comparison of Approaches

| Feature | Prop Drilling | Context API | Redux |
|---------|--------------|-------------|-------|
| Setup | None | Easy | Medium |
| Boilerplate | None | Low | Medium |
| Learning Curve | Easy | Easy | Steep |
| Performance | Best | Good | Best |
| DevTools | No | No | Excellent |
| Async Actions | Manual | Manual | Built-in |
| TypeScript | Easy | Medium | Excellent |
| Best For | Small apps | Small-medium | Large apps |

---

## When to Use Each Approach

### Use Prop Drilling when:
- Application is small
- Props are 1-2 levels deep
- State is component-specific

### Use Context when:
- Need to avoid prop drilling
- State is truly global (theme, auth, locale)
- Small to medium application
- Want to keep dependencies minimal

### Use Redux when:
- Large, complex application
- Need debugging tools
- Complex async logic
- Team prefers explicit state management
- Need middleware

---

## Other State Management Options

### Zustand
Lightweight alternative to Redux:
```typescript
import create from 'zustand';

interface UserStore {
  user: User | null;
  setUser: (user: User | null) => void;
}

const useUserStore = create<UserStore>((set) => ({
  user: null,
  setUser: (user) => set({ user })
}));

// Usage
const user = useUserStore((state) => state.user);
const setUser = useUserStore((state) => state.setUser);
```

### Jotai
Atomic state management:
```typescript
import { atom, useAtom } from 'jotai';

const userAtom = atom<User | null>(null);

function Component() {
  const [user, setUser] = useAtom(userAtom);
}
```

---

## Key Takeaways

✅ Multiple valid approaches to state management  
✅ Context API is built into React  
✅ Redux provides structure and debugging for large apps  
✅ Choose based on application size and complexity  
✅ TypeScript works well with all approaches  

---

## Next Steps

- **Chapter 9**: Fetch and manage remote data
- **Chapter 11**: Share state with custom hooks
- **Chapter 12**: Test components with state

---

## Resources

- **React Context**: https://react.dev/reference/react/createContext
- **Redux Toolkit**: https://redux-toolkit.js.org/
- **Redux DevTools**: https://github.com/reduxjs/redux-devtools
- **Zustand**: https://github.com/pmndrs/zustand
