# Learn React with TypeScript - Second Edition

[![GitHub stars](https://img.shields.io/github/stars/GizzZmo/Learn-React-with-TypeScript-2nd-Edition?style=social)](https://github.com/GizzZmo/Learn-React-with-TypeScript-2nd-Edition/stargazers)
[![GitHub forks](https://img.shields.io/github/forks/GizzZmo/Learn-React-with-TypeScript-2nd-Edition?style=social)](https://github.com/GizzZmo/Learn-React-with-TypeScript-2nd-Edition/network/members)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](http://makeapullrequest.com)

<a href="https://www.packtpub.com/product/learn-react-with-typescript-second-edition/9781804614204"><img src="https://static.packt-cdn.com/products/9781804614204/cover/smaller" alt="Learn React with TypeScript - Second Edition" height="256px" align="right"></a>

This is the code repository for [Learn React with TypeScript - Second Edition](https://www.packtpub.com/product/learn-react-with-typescript-second-edition/9781804614204), published by Packt.

**A beginner's guide to reactive web development with React 18 and TypeScript**

> 📚 **[View detailed information about this repository →](ABOUT.md)**

## Table of Contents

- [What is this book about?](#what-is-this-book-about)
- [Documentation](#-documentation)
- [Getting Started](#-getting-started)
- [Chapter Guide](#-chapter-guide)
- [What You'll Learn](#-what-youll-learn)
- [Software and Hardware List](#software-and-hardware-list)
- [Contributing](#-contributing)
- [Additional Resources](#-additional-resources)
- [Get to Know the Author](#get-to-know-the-author)
- [Download a free PDF](#download-a-free-pdf)

## What is this book about?
Reading, navigating, and debugging a large frontend codebase is a major issue faced by frontend developers. This book is designed to help web developers like you learn about ReactJS and TypeScript, both of which power large-scale apps for many organizations.

This book covers the following exciting features:
* Gain first-hand experience of TypeScript and its productivity features
* Understand how to transpile your TypeScript code into JavaScript for running in a browser
* Build a React frontend codebase with hooks
* Interact with REST and GraphQL web APIs
* Design and develop strongly typed reusable components
* Create automated component tests

If you feel this book is for you, get your [copy](https://www.amazon.com/Learn-React-TypeScript-Beginners-development/dp/1804614203/ref=tmm_pap_swatch_0?_encoding=UTF8&qid=&sr=) today!

## 📚 Documentation

This repository includes comprehensive documentation to help you learn and use the examples:

- **[ABOUT.md](ABOUT.md)** - Detailed information about this repository, learning paths, and contribution guidelines
- **[QUICK_START.md](QUICK_START.md)** - Get started quickly with installation, running examples, and troubleshooting
- **[EXAMPLES.md](EXAMPLES.md)** - Complete guide to all examples in this repository
- **Chapter READMEs** - Detailed documentation for each chapter (see below)

## 🚀 Getting Started

New to the repository? Here's your path to success:

1. **Learn About This Project**: Read the [ABOUT.md](ABOUT.md) for context and learning paths
2. **Quick Start**: Follow the [QUICK_START.md](QUICK_START.md) guide for immediate setup
3. **Browse Examples**: Check out [EXAMPLES.md](EXAMPLES.md) for a complete overview
4. **Explore Chapters**: Navigate to any chapter folder and read its README
5. **Join the Community**: Star the repo and engage in discussions

### Prerequisites

- Node.js (v16.0.0 or later)
- npm or yarn
- Basic knowledge of JavaScript and HTML/CSS

### Running an Example

```bash
# Navigate to any chapter example
cd Chapter4/Section1-Using-the-effect-hook

# Install dependencies
npm install

# Start the development server
npm start
```

## 📖 Chapter Guide

Each chapter has its own comprehensive README with detailed explanations, code examples, and best practices:

| Chapter | Topic | README |
|---------|-------|--------|
| Chapter 1 | Introducing React | [View README](Chapter1/README.md) |
| Chapter 2 | Introducing TypeScript | [View README](Chapter2/README.md) |
| Chapter 3 | Setting Up React and TypeScript | [View README](Chapter3/README.md) |
| Chapter 4 | Using React Hooks | [View README](Chapter4/README.md) |
| Chapter 5 | Approaches to Styling React Components | [View README](Chapter5/README.md) |
| Chapter 6 | Routing with React Router | [View README](Chapter6/README.md) |
| Chapter 7 | Working with Forms | [View README](Chapter7/README.md) |
| Chapter 8 | State Management | [View README](Chapter8/README.md) |
| Chapter 9 | Interacting with RESTful APIs | [View README](Chapter9/README.md) |
| Chapter 10 | Interacting with GraphQL APIs | [View README](Chapter10/README.md) |
| Chapter 11 | Reusable Components | [View README](Chapter11/README.md) |
| Chapter 12 | Unit Testing with Jest and React Testing Library | [View README](Chapter12/README.md) |

## Instructions and Navigations
All of the code is organized into folders. For example, Chapter02.

The code will look like the following:
```
class Product {
  constructor(public name: string, public unitPrice:
    number) {
    this.name = name;
    this.unitPrice = unitPrice;
  }
}

```

## 💡 What You'll Learn

By working through the examples in this repository, you'll learn:

- **React Fundamentals**: Components, JSX, props, state, and events
- **TypeScript Basics**: Types, interfaces, generics, and type safety
- **React Hooks**: useState, useEffect, useContext, and custom hooks
- **Styling**: CSS, CSS Modules, CSS-in-JS, and Tailwind CSS
- **Routing**: React Router for multi-page applications
- **Forms**: Controlled/uncontrolled components and validation
- **State Management**: Context API and Redux
- **Data Fetching**: REST APIs with fetch and React Query
- **GraphQL**: Apollo Client and GraphQL queries/mutations
- **Reusable Components**: Generic props, render props, and custom hooks
- **Testing**: Jest and React Testing Library

**Following is what you need for this book:**
This book is for experienced frontend developers looking to build large scale web applications using React and TypeScript. Intermediate knowledge of JavaScript, HTML and CSS is a prerequisite.

With the following software and hardware list you can run all code files present in the book (Chapter 1-12).

### Software and Hardware List
| Chapter | Software/Hardware required | OS required |
| -------- | ------------------------------------ | ----------------------------------- |
| 1-12 | Google Chrome | Windows, Mac OS X, and Linux |
| 1-12 | Node.js and npm | Windows, Mac OS X, and Linux |
| 1-12 | Visual Studio Code | Windows, Mac OS X, and Linux |
| 1-12 | React 18.0 or later | Windows, Mac OS X, and Linux |
| 1-12 | TypeScript 4.7 or later | Windows, Mac OS X, and Linux |

We also provide a PDF file that has color images of the screenshots/diagrams used in this book. [Click here to download it](https://packt.link/5CvU5).

## Errata
* Page 137: _In Step 7,_ React.tsx _should be_ Reset.tsx
* Page 302, _In Step 5,_
```
if '!('ti'le' in post)) {
  throw new Err"r("post do'sn't contain ti"le");
}
if (typeof post.title !'= 'str'ng') {
  throw new Err'r('title is not a str'ng');
}
if '!('descript'on' in post)) {
  throw new Err"r("post do'sn't contain descript"on");
}
if (typeof post.description !'= 'str'ng') {
  throw new Err'r('description is not a str'ng');
}
```
_should be_

```
if (!("title" in post)) {
  throw new Error("post doesn't contain title");
}
if (typeof post.title !== "string") {
  throw new Error("title is not a string");
}
if (!("description" in post)) {
  throw new Error("post doesn't contain description");
}
if (typeof post.description !== "string") {
  throw new Error("description is not a string");
}
```

## 🤝 Contributing

Found an issue or have a suggestion? Contributions are welcome!

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/improvement`)
3. Commit your changes (`git commit -am 'Add new feature'`)
4. Push to the branch (`git push origin feature/improvement`)
5. Open a Pull Request

## 📚 Additional Resources

- **React Documentation**: https://react.dev
- **TypeScript Documentation**: https://www.typescriptlang.org
- **React Router**: https://reactrouter.com
- **Redux Toolkit**: https://redux-toolkit.js.org
- **TanStack Query (React Query)**: https://tanstack.com/query
- **Testing Library**: https://testing-library.com

### Related products
* React and React Native - Fourth Edition [[Packt]](https://www.packtpub.com/product/react-and-react-native-fourth-edition/9781803231280) [[Amazon]](https://www.amazon.com/React-Native-cross-platform-JavaScript-applications/dp/1803231289)

* React Application Architecture for Production [[Packt]](https://www.packtpub.com/product/react-application-architecture-for-production/9781801070539) [[Amazon]](https://www.amazon.com/React-Application-Architecture-Production-enterprise-ready/dp/1801070539/ref=tmm_pap_swatch_0?_encoding=UTF8&qid=&sr=)


## 🙏 Acknowledgments

This repository and the accompanying book would not be possible without:

- **The React Team** at Meta for creating and maintaining React
- **The TypeScript Team** at Microsoft for TypeScript
- **The Open Source Community** for the incredible ecosystem of tools and libraries
- **All Contributors** who have helped improve this repository
- **Packt Publishing** for supporting this project
- **Readers and Learners** who provide feedback and help make this resource better

Special thanks to everyone who has starred, forked, or contributed to this repository!

## Get to Know the Author

**Carl Rippon** 
has been in the software industry for over 20 years developing complex lines of business applications in various sectors. He has spent the last 8 years building single-page applications using a wide range of JavaScript technologies including Angular, ReactJS, and TypeScript. He has also written over 100 blog posts on various technologies.

## Other books by the author
* [ASP.NET Core 5 and React - Second Edition](https://www.packtpub.com/product/aspnet-core-5-and-react-second-edition/9781800206168)
* [Learn React with TypeScript 3](https://www.packtpub.com/product/learn-react-with-typescript-3/9781789610253?_ga=2.115218816.60521389.1675320509-1593508983.1675068804)
* [ASP.NET Core 3 and React](https://www.packtpub.com/product/aspnet-core-3-and-react/9781789950229)

### Download a free PDF

 <i>If you have already purchased a print or Kindle version of this book, you can get a DRM-free PDF version at no cost.<br>Simply click on the link to claim your free PDF.</i>
<p align="center"> <a href="https://packt.link/free-ebook/9781804614204">https://packt.link/free-ebook/9781804614204 </a> </p>

## 💬 Support

### Getting Help

- **📖 Documentation**: Check [ABOUT.md](ABOUT.md), [QUICK_START.md](QUICK_START.md), and [EXAMPLES.md](EXAMPLES.md)
- **🐛 Issues**: Found a bug? [Open an issue](https://github.com/GizzZmo/Learn-React-with-TypeScript-2nd-Edition/issues)
- **💡 Discussions**: Have questions? [Start a discussion](https://github.com/GizzZmo/Learn-React-with-TypeScript-2nd-Edition/discussions)
- **⭐ Star this repo**: Show your support by starring this repository
- **🔀 Fork and Contribute**: Help improve the examples and documentation

### Stay Connected

If you find this repository helpful:
1. ⭐ **Star** the repository to show your support
2. 👁️ **Watch** for updates and new content
3. 🍴 **Fork** to create your own version
4. 📢 **Share** with others who might benefit

---

**Made with ❤️ by the React and TypeScript community**

*Happy coding! 🚀*
