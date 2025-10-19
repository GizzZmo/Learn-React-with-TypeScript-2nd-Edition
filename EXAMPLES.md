# Examples Guide

This guide provides a comprehensive overview of all the code examples in this repository. Each chapter contains practical examples that demonstrate key concepts for learning React with TypeScript.

## Table of Contents

- [Chapter 1: Introducing React](#chapter-1-introducing-react)
- [Chapter 2: Introducing TypeScript](#chapter-2-introducing-typescript)
- [Chapter 3: Setting Up React and TypeScript](#chapter-3-setting-up-react-and-typescript)
- [Chapter 4: Using React Hooks](#chapter-4-using-react-hooks)
- [Chapter 5: Approaches to Styling React Components](#chapter-5-approaches-to-styling-react-components)
- [Chapter 6: Routing with React Router](#chapter-6-routing-with-react-router)
- [Chapter 7: Working with Forms](#chapter-7-working-with-forms)
- [Chapter 8: State Management](#chapter-8-state-management)
- [Chapter 9: Interacting with RESTful APIs](#chapter-9-interacting-with-restful-apis)
- [Chapter 10: Interacting with GraphQL APIs](#chapter-10-interacting-with-graphql-apis)
- [Chapter 11: Reusable Components](#chapter-11-reusable-components)
- [Chapter 12: Unit Testing with Jest and React Testing Library](#chapter-12-unit-testing-with-jest-and-react-testing-library)

---

## Chapter 1: Introducing React

**Location:** `Chapter1/`

Learn the fundamentals of React including components, JSX, props, state, and events.

### Examples

1. **Understanding JSX** (`Section2-Understanding-JSX/`)
   - Basic JSX syntax and structure
   - Demonstrates how JSX compiles to JavaScript

2. **Creating a Component** (`Section3-Creating-a-component/`)
   - Basic function component creation
   - Component structure and exports

3. **Understanding Imports and Exports** (`Section4-Understanding-imports-and-exports/`)
   - ES6 module system
   - Importing and exporting components

4. **Using Props** (`Section5-Using-props/`)
   - Passing data to components via props
   - Default prop values
   - Alert component example with type and heading props

5. **Using State** (`Section6-Using-state/`)
   - Managing component state
   - State updates and re-renders

6. **Using Events** (`Section7-Using-events/`)
   - Handling user interactions
   - Event handlers in React

7. **Class Component** (`Class-component/`)
   - Traditional class-based components
   - Comparison with function components

---

## Chapter 2: Introducing TypeScript

**Location:** `Chapter2/`

Master TypeScript fundamentals including types, interfaces, and the TypeScript compiler.

### Section 1: Understanding TypeScript
- Basic TypeScript concepts
- Type safety benefits
- Example: `calculateTotalPrice.ts`

### Section 2: Basic Types
- **Understanding JavaScript Types**: JavaScript's type system
- **Using Type Annotations**: Explicit type declarations
- **Using Type Inference**: Automatic type detection
- **Using Arrays**: Typed arrays and array operations
- **Using the any Type**: Dynamic typing when needed
- **Using the unknown Type**: Type-safe alternative to any
- **Using the void Type**: Functions that don't return values
- **Using the never Type**: Functions that never return
- **Using the Date Type**: Working with dates in TypeScript

### Section 3: Creating Types
- **Creating Type Aliases**: Custom type definitions
- **Creating Interfaces**: Object type contracts
- **Creating Classes**: Object-oriented programming
- **Creating Union Types**: Multiple possible types
- **Creating Enumerations**: Named constants
- **Using Object Types**: Complex object structures

### Section 4: Using the Compiler
- TypeScript compiler configuration
- Compilation process
- Example: `src/product.ts`

---

## Chapter 3: Setting Up React and TypeScript

**Location:** `Chapter3/`

Learn how to set up a React and TypeScript project using different tools.

### Examples

1. **Creating a Project with Webpack** (`Section1-Creating-a-project-with-Webpack/`)
   - Manual setup with Webpack
   - Custom build configuration
   - Full control over tooling

2. **Creating a Project with Create React App** (`Section2-Creating-a-project-with-Create-React-App/`)
   - Quick start with CRA
   - Zero configuration setup
   - Ready-to-use development environment

---

## Chapter 4: Using React Hooks

**Location:** `Chapter4/`

Explore React Hooks for managing state, side effects, and component lifecycle.

### Examples

1. **Using the Effect Hook** (`Section1-Using-the-effect-hook/`)
   - Side effects in function components
   - Data fetching example
   - Dependencies and cleanup

2. **Using State Hooks** (`Section2-Using-state-hooks/`)
   - **useState**: Simple state management
   - **useReducer**: Complex state logic with reducers

3. **Using the Ref Hook** (`Section3-Using-the-ref-hook/`)
   - Referencing DOM elements
   - Mutable values that persist across renders

4. **Using the Memo Hook** (`Section4-Using-the-memo-hook/`)
   - Performance optimization
   - Memoizing expensive calculations

5. **Using the Callback Hook** (`Section5-Using-the-callback-hook/`)
   - Memoizing callback functions
   - Preventing unnecessary re-renders

---

## Chapter 5: Approaches to Styling React Components

**Location:** `Chapter5/`

Discover different approaches to styling React components.

### Examples

1. **Using Plain CSS** (`Section1-Using-plain-CSS/app/`)
   - Traditional CSS files
   - Class-based styling

2. **Using CSS Modules** (`Section2-Using-CSS-modules/app/`)
   - Scoped CSS with modules
   - Avoiding class name conflicts

3. **Using CSS-in-JS** (`Section3-Using-CSS-in-JS/app/`)
   - Styled components
   - Dynamic styling with JavaScript

4. **Using Tailwind CSS** (`Section4-Using-Tailwind-CSS/app/`)
   - Utility-first CSS framework
   - Rapid UI development

5. **Using SVGs** (`Section5-Using-SVGs/app/`)
   - Inline SVG components
   - Scalable vector graphics

---

## Chapter 6: Routing with React Router

**Location:** `Chapter6/`

Build a multi-page application with React Router.

### Features Demonstrated
- Route configuration
- Navigation between pages
- URL parameters
- Nested routes
- Protected routes
- Data loading with loaders

**Run the example:**
```bash
cd Chapter6
npm install
npm start
```

---

## Chapter 7: Working with Forms

**Location:** `Chapter7/`

Learn different approaches to handling forms in React.

### Examples

1. **Uncontrolled Fields** (`uncontrolled-fields/`)
   - DOM-based form handling
   - Using refs to access form values

2. **Controlled Fields** (`controlled-fields/`)
   - React state-controlled inputs
   - Real-time validation

3. **Native Validation** (`native-validation/`)
   - HTML5 form validation
   - Browser-native validation features

4. **React Hook Form** (`react-hook-form/`)
   - Popular form library
   - Performance-optimized forms
   - Easy validation

5. **React Router Form** (`react-router-form/`)
   - Forms with React Router
   - Form actions and loaders

**Run any example:**
```bash
cd Chapter7/<example-name>
npm install
npm start
```

---

## Chapter 8: State Management

**Location:** `Chapter8/`

Explore different state management approaches in React.

### Examples

1. **Prop Drilling** (`prop-drilling/`)
   - Passing props through multiple levels
   - Understanding the problem

2. **Context API** (`context/`)
   - React's built-in state management
   - Avoiding prop drilling
   - Global state with Context

3. **Redux** (`redux/`)
   - Redux Toolkit for state management
   - Actions, reducers, and store
   - Time-travel debugging

**Run any example:**
```bash
cd Chapter8/<example-name>
npm install
npm start
```

---

## Chapter 9: Interacting with RESTful APIs

**Location:** `Chapter9/`

Learn how to fetch and manage data from REST APIs.

### Examples

1. **useEffect Fetch** (`useEffect-fetch/`)
   - Basic data fetching with useEffect
   - Loading and error states

2. **React Query** (`react-query/`)
   - Powerful data fetching library
   - Caching and background updates
   - Automatic refetching

3. **React Router** (`react-router/`)
   - Route-based data fetching
   - Loaders and actions

4. **React Query and React Router** (`react-query-and-react-router/`)
   - Combining both approaches
   - Best of both worlds

**Run any example:**
```bash
cd Chapter9/<example-name>
npm install
npm start
```

---

## Chapter 10: Interacting with GraphQL APIs

**Location:** `Chapter10/`

Work with GraphQL APIs in React applications.

### Examples

1. **Understanding GraphQL Syntax** (`Understanding-GraphQL-Syntax/`)
   - GraphQL query language basics
   - Queries and mutations

2. **Using Apollo Client** (`Using-Apollo-Client/`)
   - Popular GraphQL client
   - Query and mutation components
   - Caching strategies

3. **Using React Query** (`Using-React-Query/`)
   - React Query with GraphQL
   - Alternative to Apollo Client

**Run the Apollo or React Query examples:**
```bash
cd Chapter10/<example-name>
npm install
npm start
```

---

## Chapter 11: Reusable Components

**Location:** `Chapter11/`

Build flexible and reusable React components.

### Examples

1. **Using Generic Props** (`Using-generic-props/`)
   - TypeScript generics in components
   - Type-safe reusable components

2. **Using Props Spreading** (`Using-props-spreading/`)
   - Spreading props to child components
   - Forwarding props efficiently

3. **Using Render Props** (`Using-render-props/`)
   - Render props pattern
   - Component composition

4. **Creating Custom Hooks** (`Creating-custom-hooks/`)
   - Extracting reusable logic
   - Custom hook patterns

5. **Adding Checked Functionality** (`Adding-checked-functionality/`)
   - Checkbox components
   - Controlled checkbox state

6. **Allowing Internal State to be Controlled** (`Allowing-internal-state-to-be-controlled/`)
   - Controlled vs uncontrolled components
   - Flexible component APIs

**Run any example:**
```bash
cd Chapter11/<example-name>
npm install
npm start
```

---

## Chapter 12: Unit Testing with Jest and React Testing Library

**Location:** `Chapter12/`

Write effective tests for React components.

### Examples

1. **Start** (`start/`)
   - Initial application state
   - Before tests are added

2. **Finish** (`finish/`)
   - Complete test suite
   - Various testing patterns
   - Component testing
   - Integration testing

**Run tests:**
```bash
cd Chapter12/finish
npm install
npm test
```

---

## Getting Started

### Prerequisites

- **Node.js** (v16 or later recommended)
- **npm** or **yarn**
- A code editor (VS Code recommended)

### Running Examples

Most examples are standalone React applications. To run an example:

1. Navigate to the example directory:
   ```bash
   cd Chapter<N>/<example-name>
   ```

2. Install dependencies:
   ```bash
   npm install
   ```

3. Start the development server:
   ```bash
   npm start
   ```

4. Open your browser to `http://localhost:3000`

### Simple Code Snippets

For simple TypeScript examples in Chapter 1 and Chapter 2, you can:

1. Install TypeScript globally:
   ```bash
   npm install -g typescript
   ```

2. Run a TypeScript file:
   ```bash
   tsc filename.ts
   node filename.js
   ```

---

## Additional Resources

- **Book**: [Learn React with TypeScript - Second Edition](https://www.packtpub.com/product/learn-react-with-typescript-second-edition/9781804614204)
- **React Documentation**: https://react.dev
- **TypeScript Documentation**: https://www.typescriptlang.org
- **React Router Documentation**: https://reactrouter.com
- **React Query Documentation**: https://tanstack.com/query

---

## Contributing

Found an issue or want to improve an example? Feel free to open an issue or submit a pull request.

## License

See the [LICENSE](LICENSE) file for details.
