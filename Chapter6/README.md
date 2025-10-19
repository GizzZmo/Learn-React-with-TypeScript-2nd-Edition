# Chapter 6: Routing with React Router

This chapter demonstrates how to build a multi-page application using React Router, the standard routing library for React.

## What You'll Learn

- Setting up React Router
- Creating routes and navigation
- URL parameters and query strings
- Nested routes
- Programmatic navigation
- Route loaders for data fetching
- Protected routes

## Running the Example

This chapter contains a complete routing application:

```bash
cd Chapter6
npm install
npm start
```

Open http://localhost:3000 to see the application.

## Project Structure

```
Chapter6/
├── src/
│   ├── pages/           # Page components
│   ├── data/           # Data and API functions
│   ├── Routes.tsx      # Route configuration
│   ├── Header.tsx      # Navigation component
│   ├── App.tsx         # Main app component
│   └── index.tsx       # Entry point
├── package.json
└── tsconfig.json
```

## Key Concepts

### Installing React Router

```bash
npm install react-router-dom
npm install --save-dev @types/react-router-dom
```

### Basic Setup

**index.tsx:**
```typescript
import { BrowserRouter } from 'react-router-dom';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')!).render(
  <BrowserRouter>
    <App />
  </BrowserRouter>
);
```

**App.tsx:**
```typescript
import { Routes, Route } from 'react-router-dom';
import Header from './Header';
import HomePage from './pages/HomePage';
import AboutPage from './pages/AboutPage';

function App() {
  return (
    <div>
      <Header />
      <Routes>
        <Route path="/" element={<HomePage />} />
        <Route path="/about" element={<AboutPage />} />
      </Routes>
    </div>
  );
}
```

### Navigation

**Using Link Component:**
```typescript
import { Link } from 'react-router-dom';

function Header() {
  return (
    <nav>
      <Link to="/">Home</Link>
      <Link to="/about">About</Link>
      <Link to="/products">Products</Link>
    </nav>
  );
}
```

**Using NavLink for Active Links:**
```typescript
import { NavLink } from 'react-router-dom';

function Navigation() {
  return (
    <nav>
      <NavLink 
        to="/"
        className={({ isActive }) => isActive ? 'active' : ''}
      >
        Home
      </NavLink>
    </nav>
  );
}
```

### URL Parameters

**Route Definition:**
```typescript
<Route path="/products/:id" element={<ProductPage />} />
```

**Accessing Parameters:**
```typescript
import { useParams } from 'react-router-dom';

function ProductPage() {
  const { id } = useParams<{ id: string }>();
  
  return <div>Product ID: {id}</div>;
}
```

### Nested Routes with Outlet

**Layout Component:**
```typescript
import { Outlet } from 'react-router-dom';

function Layout() {
  return (
    <div>
      <Header />
      <main>
        <Outlet />  {/* Child routes render here */}
      </main>
      <Footer />
    </div>
  );
}
```

## Available Scripts

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.\
You will also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

## Key Takeaways

✅ React Router enables multi-page navigation in SPAs  
✅ Routes map URLs to components  
✅ Links and NavLinks provide navigation  
✅ URL parameters pass data through the URL  
✅ Loaders enable data fetching before rendering  
✅ Nested routes enable complex layouts  

## Next Steps

- **Chapter 7**: Build forms with routing
- **Chapter 8**: Manage state across routes
- **Chapter 9**: Fetch data on route changes

## Resources

- **React Router Docs**: https://reactrouter.com/
- **React Router Tutorial**: https://reactrouter.com/en/main/start/tutorial
- **Create React App**: https://create-react-app.dev/
- **React Documentation**: https://react.dev/
