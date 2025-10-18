# Quick Start Guide

Welcome! This guide will help you get started with the code examples from "Learn React with TypeScript - Second Edition" as quickly as possible.

## Prerequisites

Before you begin, make sure you have the following installed:

- **Node.js** (v16.0.0 or later)
  - Check version: `node --version`
  - Download from: https://nodejs.org/

- **npm** (comes with Node.js)
  - Check version: `npm --version`

- **Git** (for cloning the repository)
  - Check version: `git --version`
  - Download from: https://git-scm.com/

- **Code Editor** (VS Code recommended)
  - Download from: https://code.visualstudio.com/

## Installation

### 1. Clone the Repository

```bash
git clone https://github.com/GizzZmo/Learn-React-with-TypeScript-2nd-Edition.git
cd Learn-React-with-TypeScript-2nd-Edition
```

### 2. Choose Your Chapter

The repository is organized by chapters. Each chapter contains specific examples:

```
Learn-React-with-TypeScript-2nd-Edition/
├── Chapter1/   # React Basics
├── Chapter2/   # TypeScript Fundamentals
├── Chapter3/   # Project Setup
├── Chapter4/   # React Hooks
├── Chapter5/   # Styling
├── Chapter6/   # React Router
├── Chapter7/   # Forms
├── Chapter8/   # State Management
├── Chapter9/   # REST APIs
├── Chapter10/  # GraphQL APIs
├── Chapter11/  # Reusable Components
└── Chapter12/  # Testing
```

## Running Your First Example

Let's run a complete React application from Chapter 4:

### Step 1: Navigate to the Example

```bash
cd Chapter4/Section1-Using-the-effect-hook
```

### Step 2: Install Dependencies

```bash
npm install
```

This will download all required packages. It may take a few minutes the first time.

### Step 3: Start the Development Server

```bash
npm start
```

The application will automatically open in your browser at `http://localhost:3000`.

### Step 4: Make Changes

Try editing the source files in the `src/` directory. The page will automatically reload when you save changes.

### Step 5: Stop the Server

Press `Ctrl+C` in the terminal to stop the development server.

## Quick Reference by Learning Goal

### I want to learn React basics
→ Start with **Chapter 1**
- Simple JavaScript examples
- No installation needed for most examples
- Copy code to an online editor like CodeSandbox

### I want to learn TypeScript
→ Start with **Chapter 2**

**Install TypeScript globally:**
```bash
npm install -g typescript
```

**Run a TypeScript example:**
```bash
cd Chapter2/Section2-Basic-types/Using-arrays
tsc code.ts
node code.js
```

### I want to build a full React app
→ Start with **Chapter 3**

**Create React App example:**
```bash
cd Chapter3/Section2-Creating-a-project-with-Create-React-App/myapp
npm install
npm start
```

### I want to learn React Hooks
→ Start with **Chapter 4**

**Each hook has its own example:**
```bash
# useEffect
cd Chapter4/Section1-Using-the-effect-hook
npm install && npm start

# useState
cd Chapter4/Section2-Using-state-hooks/1-Using-useState
npm install && npm start

# useReducer
cd Chapter4/Section2-Using-state-hooks/2-Using-useReducer
npm install && npm start
```

### I want to style components
→ Start with **Chapter 5**

**Try different styling approaches:**
```bash
# Plain CSS
cd Chapter5/Section1-Using-plain-CSS/app
npm install && npm start

# CSS Modules
cd Chapter5/Section2-Using-CSS-modules/app
npm install && npm start

# Tailwind CSS (fastest to see results)
cd Chapter5/Section4-Using-Tailwind-CSS/app
npm install && npm start
```

### I want to add routing
→ Start with **Chapter 6**

```bash
cd Chapter6
npm install
npm start
```

### I want to work with forms
→ Start with **Chapter 7**

**Best starting point - React Hook Form:**
```bash
cd Chapter7/react-hook-form
npm install
npm start
```

### I want to manage state
→ Start with **Chapter 8**

**Start with Context API:**
```bash
cd Chapter8/context
npm install
npm start
```

**Then try Redux:**
```bash
cd Chapter8/redux
npm install
npm start
```

### I want to fetch data from APIs
→ Start with **Chapter 9**

**Best starting point - React Query:**
```bash
cd Chapter9/react-query
npm install
npm start
```

### I want to work with GraphQL
→ Start with **Chapter 10**

```bash
cd Chapter10/Using-Apollo-Client
npm install
npm start
```

### I want to build reusable components
→ Start with **Chapter 11**

**Custom Hooks example:**
```bash
cd Chapter11/Creating-custom-hooks
npm install
npm start
```

### I want to write tests
→ Start with **Chapter 12**

```bash
cd Chapter12/finish
npm install
npm test
```

## Common Commands

Once you're in an example directory with a `package.json` file:

| Command | Purpose |
|---------|---------|
| `npm install` | Install all dependencies |
| `npm start` | Start development server |
| `npm test` | Run tests (if available) |
| `npm run build` | Build for production |
| `npm run lint` | Check code style (if configured) |

## Troubleshooting

### Port Already in Use

If port 3000 is already in use, you'll see an error. Either:
- Stop the other application using port 3000
- Choose a different port when prompted (type `y` and press Enter)

### Module Not Found Errors

If you see "Module not found" errors:
1. Delete `node_modules` folder: `rm -rf node_modules`
2. Delete `package-lock.json`: `rm package-lock.json`
3. Reinstall: `npm install`

### TypeScript Errors

If you see TypeScript errors when running examples:
1. Make sure you're in the correct directory
2. Check that `node_modules` is installed
3. Try restarting the development server

### Slow Installation

If `npm install` is slow:
- Make sure you have a good internet connection
- Try using a different npm registry
- Consider using `yarn` instead: `yarn install`

## VS Code Tips

If you're using VS Code:

### Recommended Extensions

Install these extensions for the best experience:
- **ES7+ React/Redux/React-Native snippets**: Code snippets
- **Prettier**: Code formatter
- **ESLint**: Code linting
- **TypeScript Vue Plugin (Volar)**: Better TypeScript support

### Open Multiple Examples

You can open multiple examples in VS Code workspaces:
1. File → Add Folder to Workspace
2. Select each example folder you want to work with
3. File → Save Workspace As

### Integrated Terminal

Use VS Code's integrated terminal:
- View → Terminal (or `` Ctrl+` ``)
- You can have multiple terminals open for different examples

## Online Alternatives

Don't want to install locally? Try these online editors:

- **CodeSandbox**: https://codesandbox.io/
  - Import directly from GitHub
  - Full React and TypeScript support

- **StackBlitz**: https://stackblitz.com/
  - WebContainer technology
  - Runs Node.js in the browser

- **TypeScript Playground**: https://www.typescriptlang.org/play
  - Perfect for Chapter 2 examples
  - No setup required

## Next Steps

1. **Read the Book**: The examples make the most sense when following along with the book chapters
2. **Experiment**: Modify the examples to understand how things work
3. **Build Your Own**: Use the examples as templates for your own projects
4. **Check EXAMPLES.md**: Detailed guide to all examples in the repository

## Getting Help

- **Issues**: Found a bug? Open an issue on GitHub
- **Discussions**: Have questions? Start a discussion on GitHub
- **Documentation**: Check the README.md in each example directory

## Resources

- **React Docs**: https://react.dev
- **TypeScript Docs**: https://www.typescriptlang.org
- **Book**: [Learn React with TypeScript - Second Edition](https://www.packtpub.com/product/learn-react-with-typescript-second-edition/9781804614204)

Happy coding! 🚀
