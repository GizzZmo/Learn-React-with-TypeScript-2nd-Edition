# Chapter 2: Introducing TypeScript

This chapter introduces TypeScript fundamentals, including basic types, creating custom types, interfaces, and using the TypeScript compiler.

## What You'll Learn

- TypeScript's type system and benefits
- Basic TypeScript types (string, number, boolean, arrays, etc.)
- Creating custom types with type aliases and interfaces
- Working with classes and enums
- Using the TypeScript compiler
- Type inference and type annotations

## Prerequisites

To run the examples in this chapter, install TypeScript globally:

```bash
npm install -g typescript
```

Verify installation:
```bash
tsc --version
```

## Examples Overview

### Section 1: Understanding TypeScript

**Location:** `Section1-Understanding-TypeScript/`

Introduction to TypeScript and its benefits over plain JavaScript.

**Files:**
- `calculateTotalPrice.js` - JavaScript version
- `calculateTotalPrice.ts` - TypeScript version

**Key Concepts:**
- Type safety prevents runtime errors
- Better IDE support with autocomplete
- Self-documenting code with types
- Easier refactoring

**Run the example:**
```bash
cd Section1-Understanding-TypeScript
# Compare JavaScript and TypeScript versions
tsc calculateTotalPrice.ts
node calculateTotalPrice.js
```

---

## Section 2: Basic Types

### Understanding JavaScript Types

**Location:** `Section2-Basic-types/Understanding-JavaScript-types/`

Learn about JavaScript's primitive types and how TypeScript enhances them.

### Using Type Annotations

**Location:** `Section2-Basic-types/Using-type-annotations/`

**Example:**
```typescript
let firstName: string = "John";
let age: number = 30;
let isActive: boolean = true;
```

**Key Concepts:**
- Explicit type declarations
- Syntax: `variableName: type = value`
- Catch type errors at compile time

### Using Type Inference

**Location:** `Section2-Basic-types/Using-type-inference/`

**Example:**
```typescript
let firstName = "John";  // TypeScript infers: string
let age = 30;            // TypeScript infers: number
```

**Key Concepts:**
- TypeScript automatically determines types
- Reduces boilerplate while maintaining safety
- Works best with initialization

### Using Arrays

**Location:** `Section2-Basic-types/Using-arrays/`

**File:** `code.ts`

**Examples:**
```typescript
// Array type annotation
const numbers: number[] = [1, 2, 3];
// or
const numbers: Array<number> = [1, 2, 3];

// Array methods are type-safe
numbers.push(4);      // ✓ OK
numbers.push("five"); // ✗ Error: Argument of type 'string'
```

**Run it:**
```bash
cd Section2-Basic-types/Using-arrays
tsc code.ts
node code.js
```

### Using the any Type

**Location:** `Section2-Basic-types/Using-the-any-type/`

**Key Concepts:**
- Opts out of type checking
- Use sparingly
- Useful for gradual migration from JavaScript
- Loss of type safety

**Example:**
```typescript
let value: any = "hello";
value = 123;     // OK
value = true;    // OK
```

### Using the unknown Type

**Location:** `Section2-Basic-types/Using-the-unknown-type/`

**Key Concepts:**
- Type-safe alternative to `any`
- Requires type checking before use
- Better than `any` for unknown types

**Example:**
```typescript
let value: unknown = "hello";
// value.toUpperCase(); // Error: must check type first

if (typeof value === "string") {
  value.toUpperCase(); // OK
}
```

### Using the void Type

**Location:** `Section2-Basic-types/Using-the-void-type/`

**Key Concepts:**
- Used for functions that don't return a value
- Different from `undefined`

**Example:**
```typescript
function logMessage(message: string): void {
  console.log(message);
  // No return statement
}
```

### Using the never Type

**Location:** `Section2-Basic-types/Using-the-never-type/`

**Key Concepts:**
- Represents values that never occur
- Used for functions that never return
- Useful for exhaustive type checking

**Example:**
```typescript
function throwError(message: string): never {
  throw new Error(message);
}

function infiniteLoop(): never {
  while (true) {}
}
```

### Using the Date Type

**Location:** `Section2-Basic-types/Using-the-Date-type/`

**Key Concepts:**
- Working with dates in TypeScript
- Date methods and type safety

**Example:**
```typescript
const today: Date = new Date();
const timestamp: number = today.getTime();
```

---

## Section 3: Creating Types

### Creating Type Aliases

**Location:** `Section3-Creating-types/Creating-type-aliases/`

**File:** `code.ts`

**Key Concepts:**
- Define custom types
- Reusable type definitions
- Better than repeating complex types

**Example:**
```typescript
type UserId = string | number;
type User = {
  id: UserId;
  name: string;
  email: string;
};

const user: User = {
  id: 1,
  name: "John Doe",
  email: "john@example.com"
};
```

### Creating Interfaces

**Location:** `Section3-Creating-types/Creating-interfaces/`

**File:** `code.ts`

**Key Concepts:**
- Define object shapes
- Can be extended
- Can define function signatures
- Optional properties with `?`

**Example:**
```typescript
interface Product {
  name: string;
  unitPrice?: number;  // Optional
  purchase: (quantity: number) => void;
}

const table: Product = {
  name: "Table",
  purchase: (quantity) => console.log(`Purchased ${quantity} tables`)
};
```

**Interface Extension:**
```typescript
interface DiscountedProduct extends Product {
  discount: number;
}
```

**Run it:**
```bash
cd Section3-Creating-types/Creating-interfaces
tsc code.ts
node code.js
```

### Creating Classes

**Location:** `Section3-Creating-types/Creating-classes/`

**File:** `code.ts`

**Key Concepts:**
- Object-oriented programming in TypeScript
- Access modifiers (public, private, protected)
- Constructor shorthand
- Methods and properties

**Example:**
```typescript
class Product {
  constructor(
    public name: string,
    public unitPrice: number
  ) {}
  
  getTotal(quantity: number): number {
    return this.unitPrice * quantity;
  }
}

const product = new Product("Table", 500);
console.log(product.getTotal(2)); // 1000
```

### Creating Union Types

**Location:** `Section3-Creating-types/Creating-union-types/`

**File:** `code.ts`

**Key Concepts:**
- A value can be one of several types
- Use `|` operator
- Type narrowing with type guards

**Example:**
```typescript
type Status = "pending" | "approved" | "rejected";
type Id = string | number;

function processId(id: Id) {
  if (typeof id === "string") {
    console.log(id.toUpperCase());
  } else {
    console.log(id.toFixed(2));
  }
}
```

### Creating Enumerations

**Location:** `Section3-Creating-types/Creating-enumerations/`

**File:** `code.ts`

**Key Concepts:**
- Named constants
- Numeric or string enums
- Better than magic numbers/strings

**Example:**
```typescript
enum Status {
  Pending = "PENDING",
  Approved = "APPROVED",
  Rejected = "REJECTED"
}

let orderStatus: Status = Status.Pending;

// Numeric enums
enum Direction {
  Up,    // 0
  Down,  // 1
  Left,  // 2
  Right  // 3
}
```

### Using Object Types

**Location:** `Section3-Creating-types/Using-object-types/`

**File:** `code.ts`

**Key Concepts:**
- Inline object types
- Index signatures
- Nested objects

**Example:**
```typescript
const user: {
  id: number;
  name: string;
  address: {
    street: string;
    city: string;
  };
} = {
  id: 1,
  name: "John",
  address: {
    street: "123 Main St",
    city: "Boston"
  }
};
```

---

## Section 4: Using the Compiler

**Location:** `Section4-Using-the-compiler/`

Learn how to configure and use the TypeScript compiler.

**Files:**
- `src/product.ts` - TypeScript source
- `tsconfig.json` - Compiler configuration (if present)

**Key Concepts:**
- `tsc` command compiles TypeScript to JavaScript
- `tsconfig.json` configures compilation options
- Watch mode for automatic compilation
- Source maps for debugging

### Basic Commands

```bash
# Compile a single file
tsc product.ts

# Compile with watch mode
tsc product.ts --watch

# Compile entire project (with tsconfig.json)
tsc

# Check for errors without emitting files
tsc --noEmit
```

### Common tsconfig.json Options

```json
{
  "compilerOptions": {
    "target": "ES2020",           // JavaScript version to compile to
    "module": "commonjs",          // Module system
    "strict": true,                // Enable all strict type checking
    "esModuleInterop": true,       // Better module compatibility
    "skipLibCheck": true,          // Skip type checking of declaration files
    "forceConsistentCasingInFileNames": true,
    "outDir": "./dist",            // Output directory
    "rootDir": "./src",            // Source directory
    "sourceMap": true              // Generate .map files for debugging
  },
  "include": ["src/**/*"],
  "exclude": ["node_modules"]
}
```

## How to Run Examples

Most examples in this chapter are simple TypeScript files. To run them:

### Method 1: Compile and Run
```bash
# Navigate to example directory
cd Section3-Creating-types/Creating-interfaces

# Compile TypeScript to JavaScript
tsc code.ts

# Run the JavaScript output
node code.js
```

### Method 2: Use ts-node (faster for testing)
```bash
# Install ts-node globally
npm install -g ts-node

# Run TypeScript directly
ts-node code.ts
```

### Method 3: TypeScript Playground
Use the online TypeScript Playground for quick experimentation:
https://www.typescriptlang.org/play

## Key Takeaways

By the end of this chapter, you should understand:

✅ TypeScript's type system and benefits  
✅ Basic types (string, number, boolean, arrays, etc.)  
✅ Type annotations vs type inference  
✅ Creating custom types with type aliases  
✅ Defining object shapes with interfaces  
✅ Working with classes and OOP concepts  
✅ Using union types and enums  
✅ Compiling TypeScript to JavaScript  

## Common Type Patterns

### Function Types
```typescript
// Function with typed parameters and return type
function add(a: number, b: number): number {
  return a + b;
}

// Function type alias
type MathOperation = (a: number, b: number) => number;

// Arrow function with types
const multiply: MathOperation = (a, b) => a * b;
```

### Optional and Default Parameters
```typescript
function greet(name: string, greeting: string = "Hello"): string {
  return `${greeting}, ${name}!`;
}

function formatUser(name: string, age?: number): string {
  return age ? `${name} (${age})` : name;
}
```

### Type Guards
```typescript
function processValue(value: string | number) {
  if (typeof value === "string") {
    return value.toUpperCase();
  } else {
    return value.toFixed(2);
  }
}
```

## Next Steps

- **Chapter 3**: Set up a React project with TypeScript
- **Chapter 4**: Use TypeScript with React Hooks
- Apply these TypeScript concepts to React components

## Resources

- **TypeScript Documentation**: https://www.typescriptlang.org/docs/
- **TypeScript Handbook**: https://www.typescriptlang.org/docs/handbook/intro.html
- **TypeScript Playground**: https://www.typescriptlang.org/play
- **Type Challenges**: https://github.com/type-challenges/type-challenges

## Troubleshooting

### "tsc: command not found"
Install TypeScript globally:
```bash
npm install -g typescript
```

### Compilation Errors
Check your TypeScript version and ensure code syntax is correct:
```bash
tsc --version
```

### Module Errors
If you see module resolution errors, check your `tsconfig.json` or compile with specific module options:
```bash
tsc --module commonjs code.ts
```
