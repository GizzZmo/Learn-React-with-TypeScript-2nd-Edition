# Chapter 1: Introducing React

This chapter introduces the fundamental concepts of React, including components, JSX, props, state, and event handling.

## What You'll Learn

- Understanding JSX syntax
- Creating React components
- Importing and exporting modules
- Using props to pass data
- Managing component state
- Handling events in React
- Comparing class and function components

## Examples Overview

### Section 2: Understanding JSX

**Location:** `Section2-Understanding-JSX/`

JSX is a syntax extension for JavaScript that allows you to write HTML-like code in your JavaScript files.

**File:** `JSX-snippet.txt`

**Key Concepts:**
- JSX compiles to JavaScript
- JSX elements must have a single parent
- JavaScript expressions in JSX use curly braces `{}`
- JSX attributes use camelCase

### Section 3: Creating a Component

**Location:** `Section3-Creating-a-component/`

Learn how to create a basic React function component.

**File:** `Alert.js`

**Key Concepts:**
- Function components are JavaScript functions that return JSX
- Components can be reused throughout your application
- Component names should start with a capital letter

**Example:**
```javascript
export function Alert() {
  return (
    <div>
      <span role="img" aria-label="Warning">⚠</span>
      <span>Alert!</span>
    </div>
  );
}
```

### Section 4: Understanding Imports and Exports

**Location:** `Section4-Understanding-imports-and-exports/`

Master ES6 module system for organizing your React code.

**Files:** 
- `Alert.js` - Component definition with export
- `App.js` - Component usage with import

**Key Concepts:**
- `export` keyword makes components available to other files
- `import` keyword brings components into your file
- Named exports vs default exports
- Import paths (relative vs absolute)

**Example:**
```javascript
// Alert.js
export function Alert() { /* ... */ }

// App.js
import { Alert } from "./Alert";
```

### Section 5: Using Props

**Location:** `Section5-Using-props/`

Props allow you to pass data from parent to child components.

**Files:**
- `Alert.js` - Component that receives props
- `App.js` - Parent component that passes props

**Key Concepts:**
- Props are passed as attributes in JSX
- Props are read-only (immutable)
- Default prop values
- Destructuring props

**Example:**
```javascript
// Alert.js
export function Alert({ type = "information", heading, children }) {
  return (
    <div>
      <span>{type === "warning" ? "⚠" : "ℹ️"}</span>
      <span>{heading}</span>
      <div>{children}</div>
    </div>
  );
}

// App.js
<Alert type="information" heading="Success">
  Everything is really good!
</Alert>
```

**Props Used:**
- `type`: The type of alert (warning or information)
- `heading`: The alert title
- `children`: The alert message content

### Section 6: Using State

**Location:** `Section6-Using-state/`

State allows components to manage their own data that can change over time.

**Files:**
- `Alert.js` - Component with state
- `App.js` - Application entry point

**Key Concepts:**
- State is mutable and managed within a component
- The `useState` hook for function components
- State updates trigger re-renders
- Each component has its own state

**Example Pattern:**
```javascript
import { useState } from 'react';

function MyComponent() {
  const [value, setValue] = useState(initialValue);
  
  // Use value and setValue to manage state
}
```

### Section 7: Using Events

**Location:** `Section7-Using-events/`

Learn how to handle user interactions like clicks, form submissions, and more.

**Files:**
- `Alert.js` - Component with event handlers
- `App.js` - Application entry point

**Key Concepts:**
- Event handlers are functions
- React events use camelCase (e.g., `onClick`, `onChange`)
- Event handlers receive an event object
- Preventing default behavior

**Example Pattern:**
```javascript
function MyComponent() {
  const handleClick = (event) => {
    console.log('Button clicked!');
  };
  
  return <button onClick={handleClick}>Click me</button>;
}
```

### Class Component

**Location:** `Class-component/`

Understand the traditional class-based component syntax (used in older React code).

**Files:**
- `Alert.js` - Class component example
- `App.js` - Using class components

**Key Concepts:**
- Class components extend `React.Component`
- State is an object in `this.state`
- Event handlers need to be bound to `this`
- `render()` method returns JSX

**Note:** While class components are still supported, modern React development primarily uses function components with hooks.

## How to Use These Examples

These are code snippets designed to illustrate specific concepts. You can:

### Option 1: View and Study
Simply read the code files to understand the concepts being demonstrated.

### Option 2: Use in an Online Editor
Copy the code to an online editor like:
- **CodeSandbox**: https://codesandbox.io/
- **StackBlitz**: https://stackblitz.com/
- **CodePen**: https://codepen.io/

### Option 3: Create a Full React App
To see these examples running in a complete application:

1. Create a new React app:
```bash
npx create-react-app my-chapter1-examples
cd my-chapter1-examples
```

2. Copy the component code into your `src` folder

3. Import and use in your `App.js`

4. Run the app:
```bash
npm start
```

## Key Takeaways

By the end of this chapter, you should understand:

✅ JSX syntax and how it works  
✅ How to create function components  
✅ How to import and export components  
✅ How to pass data with props  
✅ How to manage state with `useState`  
✅ How to handle events  
✅ The difference between class and function components  

## Next Steps

- **Chapter 2**: Learn TypeScript fundamentals to add type safety to your React code
- **Chapter 3**: Set up a complete React + TypeScript project
- **Chapter 4**: Dive deeper into React Hooks

## Common Patterns Demonstrated

### Component Structure
```javascript
// 1. Import dependencies
import { useState } from 'react';

// 2. Define component
export function MyComponent({ prop1, prop2 }) {
  // 3. Hooks (state, effects, etc.)
  const [state, setState] = useState(initialValue);
  
  // 4. Event handlers and functions
  const handleClick = () => { /* ... */ };
  
  // 5. Return JSX
  return (
    <div>
      <p>{prop1}</p>
      <button onClick={handleClick}>Click</button>
    </div>
  );
}
```

### Props and Children
```javascript
// Parent component
<Alert type="warning" heading="Warning">
  This is the child content
</Alert>

// Child component
function Alert({ type, heading, children }) {
  return (
    <div>
      <h3>{heading}</h3>
      <div>{children}</div>
    </div>
  );
}
```

## Resources

- **React Documentation**: https://react.dev
- **React Tutorial**: https://react.dev/learn
- **JSX In Depth**: https://react.dev/learn/writing-markup-with-jsx

## Questions?

If you have questions about these examples, refer to the book "Learn React with TypeScript - Second Edition" for detailed explanations and context.
