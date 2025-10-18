# Chapter 5: Approaches to Styling React Components

This chapter explores different ways to style React components, from traditional CSS to modern CSS-in-JS solutions.

## What You'll Learn

- Using plain CSS files
- Scoping styles with CSS Modules
- Writing CSS in JavaScript with styled-components
- Utility-first styling with Tailwind CSS
- Working with SVG components

## Examples Overview

All examples in this chapter are complete React applications demonstrating different styling approaches.

---

## Section 1: Using Plain CSS

**Location:** `Section1-Using-plain-CSS/app/`

Traditional CSS styling with separate CSS files.

### Running the Example

```bash
cd Section1-Using-plain-CSS/app
npm install
npm start
```

### Key Concepts

**Structure:**
```
src/
├── App.tsx
├── App.css          # Component styles
├── index.tsx
└── index.css        # Global styles
```

**Example:**
```typescript
// App.tsx
import './App.css';

function App() {
  return <div className="container">Hello</div>;
}
```

```css
/* App.css */
.container {
  padding: 20px;
  background-color: #f0f0f0;
}
```

### Advantages

✅ Familiar and simple  
✅ No additional dependencies  
✅ Easy to understand  
✅ Good browser support  

### Disadvantages

❌ No scoping (styles are global)  
❌ Risk of class name conflicts  
❌ Hard to maintain in large apps  
❌ No dynamic styling  

---

## Section 2: Using CSS Modules

**Location:** `Section2-Using-CSS-modules/app/`

CSS Modules automatically scope CSS to components, preventing style conflicts.

### Running the Example

```bash
cd Section2-Using-CSS-modules/app
npm install
npm start
```

### Key Concepts

**File Naming:**
- Use `.module.css` extension
- Example: `App.module.css`

**Usage:**
```typescript
// App.tsx
import styles from './App.module.css';

function App() {
  return (
    <div className={styles.container}>
      <h1 className={styles.title}>Hello</h1>
    </div>
  );
}
```

```css
/* App.module.css */
.container {
  padding: 20px;
}

.title {
  color: blue;
}
```

**What Happens:**
CSS Modules generates unique class names:
```html
<div class="App_container__abc123">
  <h1 class="App_title__def456">Hello</h1>
</div>
```

### Advanced Features

**Composition:**
```css
/* Button.module.css */
.base {
  padding: 10px;
  border: none;
}

.primary {
  composes: base;
  background-color: blue;
  color: white;
}

.secondary {
  composes: base;
  background-color: gray;
  color: white;
}
```

**Global Styles:**
```css
:global(.global-class) {
  /* This won't be scoped */
}
```

### Advantages

✅ Automatic scoping  
✅ No naming conflicts  
✅ Works with standard CSS  
✅ Built into Create React App  
✅ TypeScript support available  

### Disadvantages

❌ Different syntax from standard CSS  
❌ No dynamic styling  
❌ Need to import styles in every component  

---

## Section 3: Using CSS-in-JS

**Location:** `Section3-Using-CSS-in-JS/app/`

Write CSS directly in JavaScript using styled-components or similar libraries.

### Running the Example

```bash
cd Section3-Using-CSS-in-JS/app
npm install
npm start
```

### Key Concepts

This example likely uses styled-components or Emotion.

**With styled-components:**
```typescript
import styled from 'styled-components';

const Container = styled.div`
  padding: 20px;
  background-color: #f0f0f0;
`;

const Title = styled.h1<{ color?: string }>`
  color: ${props => props.color || 'black'};
  font-size: 24px;
`;

function App() {
  return (
    <Container>
      <Title color="blue">Hello World</Title>
    </Container>
  );
}
```

### Dynamic Styling

```typescript
const Button = styled.button<{ primary?: boolean }>`
  padding: 10px 20px;
  background-color: ${props => props.primary ? 'blue' : 'gray'};
  color: white;
  border: none;
  cursor: pointer;
  
  &:hover {
    opacity: 0.8;
  }
`;

// Usage
<Button primary>Primary</Button>
<Button>Secondary</Button>
```

### Theme Support

```typescript
import { ThemeProvider } from 'styled-components';

const theme = {
  colors: {
    primary: 'blue',
    secondary: 'gray'
  }
};

const Button = styled.button`
  background-color: ${props => props.theme.colors.primary};
`;

function App() {
  return (
    <ThemeProvider theme={theme}>
      <Button>Themed Button</Button>
    </ThemeProvider>
  );
}
```

### Advantages

✅ Scoped by default  
✅ Dynamic styling with props  
✅ Theme support  
✅ No separate CSS files  
✅ Full JavaScript power  
✅ TypeScript support  

### Disadvantages

❌ Additional dependency  
❌ Learning curve  
❌ Bundle size increase  
❌ Runtime styling overhead  

---

## Section 4: Using Tailwind CSS

**Location:** `Section4-Using-Tailwind-CSS/app/`

Utility-first CSS framework for rapid UI development.

### Running the Example

```bash
cd Section4-Using-Tailwind-CSS/app
npm install
npm start
```

### Key Concepts

**Utility Classes:**
```typescript
function App() {
  return (
    <div className="p-4 bg-gray-100">
      <h1 className="text-2xl font-bold text-blue-600">
        Hello World
      </h1>
      <button className="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
        Click Me
      </button>
    </div>
  );
}
```

### Common Utilities

**Spacing:**
- `p-4` - padding: 1rem
- `m-4` - margin: 1rem
- `px-4` - padding left/right
- `mt-4` - margin top

**Colors:**
- `bg-blue-500` - background color
- `text-white` - text color
- `border-gray-300` - border color

**Layout:**
- `flex` - display: flex
- `grid` - display: grid
- `hidden` - display: none
- `block` - display: block

**Typography:**
- `text-xl` - font size
- `font-bold` - font weight
- `text-center` - text align

**Responsive Design:**
```typescript
<div className="text-sm md:text-base lg:text-lg">
  Responsive text
</div>
```

### Configuration

**tailwind.config.js:**
```javascript
module.exports = {
  content: [
    "./src/**/*.{js,jsx,ts,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        primary: '#1234AB',
      }
    },
  },
  plugins: [],
}
```

### Custom Components

```typescript
// Button.tsx
interface ButtonProps {
  variant?: 'primary' | 'secondary';
  children: React.ReactNode;
}

function Button({ variant = 'primary', children }: ButtonProps) {
  const baseClasses = "px-4 py-2 rounded font-medium";
  const variantClasses = variant === 'primary' 
    ? "bg-blue-500 text-white hover:bg-blue-600"
    : "bg-gray-200 text-gray-800 hover:bg-gray-300";
  
  return (
    <button className={`${baseClasses} ${variantClasses}`}>
      {children}
    </button>
  );
}
```

### Advantages

✅ Rapid development  
✅ No CSS files to write  
✅ Consistent design system  
✅ Responsive utilities  
✅ Production optimized (purges unused CSS)  
✅ Highly customizable  

### Disadvantages

❌ Long className strings  
❌ Learning curve for utilities  
❌ Initial setup required  
❌ HTML can become cluttered  

---

## Section 5: Using SVGs

**Location:** `Section5-Using-SVGs/app/`

Working with Scalable Vector Graphics in React.

### Running the Example

```bash
cd Section5-Using-SVGs/app
npm install
npm start
```

### Methods to Use SVGs

**1. Import as Component (recommended):**
```typescript
import { ReactComponent as Logo } from './logo.svg';

function App() {
  return <Logo className="w-20 h-20" />;
}
```

**2. Import as URL:**
```typescript
import logoUrl from './logo.svg';

function App() {
  return <img src={logoUrl} alt="Logo" />;
}
```

**3. Inline SVG:**
```typescript
function Icon() {
  return (
    <svg width="24" height="24" viewBox="0 0 24 24">
      <path d="M12 2L2 7l10 5 10-5-10-5z" />
    </svg>
  );
}
```

### SVG Components

```typescript
interface IconProps {
  size?: number;
  color?: string;
  className?: string;
}

function CheckIcon({ size = 24, color = 'currentColor', className }: IconProps) {
  return (
    <svg 
      width={size} 
      height={size} 
      viewBox="0 0 24 24"
      className={className}
    >
      <path 
        fill={color}
        d="M9 16.17L4.83 12l-1.42 1.41L9 19 21 7l-1.41-1.41z"
      />
    </svg>
  );
}

// Usage
<CheckIcon size={32} color="green" />
```

### Animated SVGs

```typescript
const AnimatedCircle = styled.circle`
  animation: pulse 2s infinite;
  
  @keyframes pulse {
    0%, 100% { r: 10; }
    50% { r: 15; }
  }
`;

function AnimatedIcon() {
  return (
    <svg width="100" height="100">
      <AnimatedCircle cx="50" cy="50" fill="blue" />
    </svg>
  );
}
```

### Advantages

✅ Scalable (resolution independent)  
✅ Small file size  
✅ Can be styled with CSS  
✅ Accessible  
✅ Animatable  

---

## Comparison of Styling Approaches

| Feature | Plain CSS | CSS Modules | CSS-in-JS | Tailwind | SVG |
|---------|-----------|-------------|-----------|----------|-----|
| Learning Curve | Easy | Easy | Medium | Medium | Easy |
| Scoping | No | Yes | Yes | N/A | N/A |
| Dynamic Styles | No | No | Yes | Yes | Yes |
| Bundle Size | Small | Small | Medium | Small* | Small |
| Performance | Fast | Fast | Good | Fast | Fast |
| Maintenance | Hard | Medium | Easy | Easy | Easy |

*After purging unused styles

---

## Choosing the Right Approach

### Use Plain CSS when:
- Learning React
- Small projects
- Simple styling needs

### Use CSS Modules when:
- Medium projects
- Want CSS scoping
- Prefer standard CSS syntax

### Use CSS-in-JS when:
- Need dynamic styling
- Want component-scoped styles
- Building a component library

### Use Tailwind when:
- Rapid prototyping
- Want consistent design
- Building utility-first UIs

---

## Key Takeaways

✅ Multiple valid approaches to styling  
✅ Each approach has trade-offs  
✅ CSS Modules provide scoping with standard CSS  
✅ CSS-in-JS enables dynamic styling  
✅ Tailwind enables rapid development  
✅ SVGs are great for icons and graphics  

---

## Next Steps

- **Chapter 6**: Add routing to your styled application
- **Chapter 7**: Style forms with these techniques
- **Chapter 11**: Create reusable styled components

---

## Resources

- **CSS Modules**: https://github.com/css-modules/css-modules
- **styled-components**: https://styled-components.com/
- **Tailwind CSS**: https://tailwindcss.com/
- **SVG on MDN**: https://developer.mozilla.org/en-US/docs/Web/SVG
