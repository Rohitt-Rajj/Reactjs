# ⚛️ React Fundamentals — Placement Preparation Notes

---

## 📌 Table of Contents
1. [What is React?](#1-what-is-react)
2. [Setting Up React (CDN)](#2-setting-up-react-cdn)
3. [React.createElement()](#3-reactcreateelement)
4. [JSX — JavaScript XML](#4-jsx--javascript-xml)
5. [JSX Transformation Flow](#5-jsx-transformation-flow)
6. [React Element vs React Component](#6-react-element-vs-react-component)
7. [Rendering with ReactDOM](#7-rendering-with-reactdom)
8. [JSX Expressions & Rules](#8-jsx-expressions--rules)
9. [Conditional Rendering](#9-conditional-rendering)
10. [Rendering Lists](#10-rendering-lists)
11. [Inline Styles in JSX](#11-inline-styles-in-jsx)
12. [Props in React](#12-props-in-react)
13. [Destructuring Props](#13-destructuring-props)
14. [Fragments](#14-fragments)
15. [Complete Project Example](#15-complete-project-example)
16. [Important Interview Questions](#16-important-interview-questions)

---

## 1. What is React?

> **React** is a JavaScript library (developed by Meta/Facebook) for building fast, interactive **User Interfaces (UI)**.

### Key Features
- **Component-Based Architecture** — UI is broken into reusable components
- **Virtual DOM** — React keeps a virtual copy of the DOM to optimize real DOM updates
- **Declarative** — You describe *what* the UI should look like, React handles *how* to update it
- **Unidirectional Data Flow** — Data flows from parent → child via props

---

## 2. Setting Up React (CDN)

> For quick projects or learning, React can be loaded via CDN (no build tools needed).

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>React App</title>
</head>
<body style="background-color: black; color: white; height: 100vh;">
    <div id="root"></div>
</body>

<!-- React core library -->
<script crossorigin src="https://unpkg.com/react@18/umd/react.development.js"></script>

<!-- ReactDOM for browser rendering -->
<script crossorigin src="https://unpkg.com/react-dom@18/umd/react-dom.development.js"></script>

<!-- Babel: Converts JSX to JavaScript in the browser -->
<script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>

<!-- Your app (type="text/babel" so Babel processes it) -->
<script type="text/babel" src="./app.js"></script>
</html>
```

### 🔑 Why `type="text/babel"`?
The browser cannot understand JSX. Babel is a **transpiler** that converts JSX → plain JavaScript at runtime.

---

## 3. React.createElement()

> Every JSX tag is ultimately a call to `React.createElement()`.

### Syntax
```javascript
React.createElement(type, props, ...children)
```

| Parameter   | Description                                  |
|-------------|----------------------------------------------|
| `type`      | HTML tag name (string) or Component (function) |
| `props`     | Object of attributes `{id, className, ...}` or `null` |
| `children`  | Text content or nested elements              |

### Examples
```javascript
// Single element
const element = React.createElement('h1', { id: 'title' }, "Hello Coder Army");

// Nested elements
const element2 = React.createElement('div', null,
    React.createElement('h1', null, "Hello"),
    React.createElement('h2', null, "Hi")
);
```

---

## 4. JSX — JavaScript XML

> **JSX** is a syntax extension for JavaScript that looks like HTML. It makes writing React elements easier and more readable.

### JSX Example
```jsx
// JSX
const element = <h1 id="title" className="first">Hello Coder Army</h1>;

// Equivalent React.createElement call
const element = React.createElement('h1', { id: 'title', className: 'first' }, "Hello Coder Army");
```

### ⚠️ JSX vs HTML Differences

| HTML Attribute | JSX Equivalent |
|----------------|----------------|
| `class`        | `className`    |
| `for`          | `htmlFor`      |
| `onclick`      | `onClick`      |
| `style="..."`  | `style={{ }}` (object) |

---

## 5. JSX Transformation Flow

> This is one of the **most important concepts** to understand for interviews.

```
JSX  →  React.createElement()  →  React Element (JS Object)  →  Real DOM (HTML Element)
       [Babel]                   [React]                         [ReactDOM]
```

### Step-by-Step Example

**Step 1 — JSX:**
```jsx
<h1 id="title">Hello Coder Army</h1>
```

**Step 2 — After Babel (React.createElement):**
```javascript
React.createElement('h1', { id: 'title' }, "Hello Coder Army")
```

**Step 3 — React Element (Plain JS Object):**
```javascript
{
    type: "h1",
    props: {
        id: "title",
        children: "Hello Coder Army"
    }
}
```

**Step 4 — Real DOM (HTML):**
```html
<h1 id="title">Hello Coder Army</h1>
```

---

## 6. React Element vs React Component

### React Element
> A **React Element** is a plain JavaScript object describing what to render. It is **immutable** — once created, you cannot change it.

```jsx
// React Element (lowercase tag = HTML element)
const element = <h1>Hello World</h1>;
```

### React Component
> A **React Component** is a **function** (or class) that returns JSX. Components are reusable, composable UI building blocks.

```jsx
// React Component (MUST start with Uppercase)
function App() {
    return (
        <h1>Hello Coder Army {10 + 20}</h1>
    );
}
```

### ⚠️ Key Difference

| Feature          | React Element          | React Component         |
|------------------|------------------------|-------------------------|
| What it is       | Plain JS Object        | Function / Class        |
| Starts with      | lowercase (`div`, `h1`)| **Uppercase** (`App`, `Header`) |
| Reusable?        | ❌ No                  | ✅ Yes                  |
| Has logic/state? | ❌ No                  | ✅ Yes                  |

---

## 7. Rendering with ReactDOM

> **ReactDOM** connects React to the browser's real DOM.

```javascript
// Step 1: Select the root DOM node
const root = ReactDOM.createRoot(document.getElementById('root'));

// Step 2: Render the App component inside it
root.render(<App />);
```

### React 18 vs React 17
| React 17 (Old)               | React 18 (New)                        |
|------------------------------|---------------------------------------|
| `ReactDOM.render(<App/>, root)` | `ReactDOM.createRoot(root).render(<App/>)` |

---

## 8. JSX Expressions & Rules

> Inside JSX, you can embed JavaScript using `{ }` curly braces.

### What Can Be Rendered?

| Type           | Renders?       | Displays?                        |
|----------------|----------------|----------------------------------|
| Number         | ✅ Yes          | Shows the number                 |
| String         | ✅ Yes          | Shows the text                   |
| Array          | ✅ Yes          | Renders each element             |
| `true`/`false` | ✅ Yes (renders) | ❌ Nothing displayed            |
| `null`         | ✅ Yes (renders) | ❌ Nothing displayed            |
| `undefined`    | ✅ Yes (renders) | ❌ Nothing displayed            |
| Object `{}`    | ❌ **ERROR**   | Cannot render plain objects      |

```jsx
const age = 10;
const name = "Rohit";
const isLoggedIn = true;

const element = (
    <div>
        <p>{age}</p>          {/* ✅ renders: 10 */}
        <p>{name}</p>         {/* ✅ renders: Rohit */}
        <p>{isLoggedIn}</p>   {/* ✅ renders but shows nothing */}
        {/* <p>{{a:1}}</p> */} {/* ❌ ERROR: Objects are not valid as React child */}
    </div>
);
```

---

## 9. Conditional Rendering

> Use the **ternary operator** inside JSX for conditional rendering.

```jsx
const age = 20;
const isLoggedIn = true;

// Ternary operator
const element = (
    <h1>
        {isLoggedIn ? <h2>Logged In</h2> : <h2>Kindly Sign In</h2>}
    </h1>
);

// Voting eligibility
function Main({ user }) {
    return (
        <h3>
            {user.age > 18 ? "You are eligible to vote" : "You are not eligible to vote"}
        </h3>
    );
}
```

### Other Patterns
```jsx
// Short-circuit (&&): Renders only if condition is true
{isLoggedIn && <p>Welcome back!</p>}
```

---

## 10. Rendering Lists

> Use `.map()` to render arrays of elements in JSX.

```jsx
const courses = ["HTML", "CSS", "JavaScript", "React"];

const element = (
    <ul>
        {courses.map(course => <li>{course}</li>)}
    </ul>
);
```

### ⚠️ Always add `key` prop when rendering lists!

```jsx
const element = (
    <ul>
        {courses.map((course, index) => <li key={index}>{course}</li>)}
    </ul>
);
```

> **Why `key`?** React uses keys to identify which items have changed, are added, or removed. Without keys, React may re-render the entire list unnecessarily.

---

## 11. Inline Styles in JSX

> In JSX, `style` takes a **JavaScript object**, not a CSS string.

```jsx
// ✅ Correct — double curly braces {{ }}
// Outer {} = JSX expression, Inner {} = JS object
const element = (
    <h1 style={{ backgroundColor: "orange", color: "white" }}>
        Hello Coder Army
    </h1>
);

// Alternative — store style in a variable
const myStyle = { backgroundColor: "orange", color: "white" };
const element = <h1 style={myStyle}>Hello Coder Army</h1>;
```

### ⚠️ CSS vs JSX Style Property Names

| CSS              | JSX                |
|------------------|--------------------|
| `background-color` | `backgroundColor` |
| `font-size`       | `fontSize`         |
| `border-radius`   | `borderRadius`     |

---

## 12. Props in React

> **Props** (Properties) are the way to pass data from a **parent component** to a **child component**. Props are **read-only**.

```jsx
// Passing props from parent
const element = <App name="Rohit" age={30} />;

// Receiving props in child
function App(props) {
    return (
        <h1>Hello {props.name}, Age: {props.age}</h1>
    );
}
```

### Props as an Object
When you write `<App name="Rohit" age={30} />`, React internally creates:
```javascript
const props = {
    name: "Rohit",
    age: 30
};
// Then calls: App(props)
```

### Passing Objects as Props
```jsx
// Passing a user object as a prop
<Main user={{ name: "Rohit", age: 30, city: "Kotdwar" }} />

// Receiving it in the component
function Main({ user }) {
    return (
        <>
            <h2>Hi {user.name}</h2>
            <p>City: {user.city}</p>
        </>
    );
}
```

---

## 13. Destructuring Props

> Instead of using `props.name`, you can **destructure** directly in the function parameter.

```jsx
// Without destructuring
function Header(props) {
    return <h1>{props.name} Welcome!</h1>;
}

// ✅ With destructuring (cleaner)
function Header({ name }) {
    return <h1>{name} Welcome!</h1>;
}
```

---

## 14. Fragments

> A component can only return **one root element**. Use **Fragments** (`<>...</>`) to return multiple elements without adding an extra DOM node.

```jsx
// ❌ Error — multiple root elements
function Main() {
    return (
        <h2>Hello</h2>
        <p>World</p>
    );
}

// ✅ Correct — wrap in a Fragment
function Main() {
    return (
        <>
            <h2>Hello</h2>
            <p>World</p>
        </>
    );
}
```

---

## 15. Complete Project Example

> **Indian Election Commission Website** — demonstrates components, props, destructuring, conditional rendering.

### `app.js`
```jsx
// Header Component — receives name via props (destructured)
function Header({ name }) {
    return (
        <h1>{name} — Welcome to Indian Election Commission Website</h1>
    );
}

// Main Component — receives user object, uses conditional rendering
function Main({ user }) {
    return (
        <>
            <h2>Hi {user.name}</h2>
            <h3>{user.age > 18 ? "You are eligible to vote" : "You are not eligible to vote"}</h3>
            <p>Your city is {user.city}</p>
        </>
    );
}

// Footer Component — no props needed
function Footer() {
    return (
        <h3>Thanks for visiting our website</h3>
    );
}

// Root App Component — composes all components
function App() {
    return (
        <>
            <Header name="Rohit" />
            <Main user={{ name: "Rohit", age: 30, city: "Kotdwar" }} />
            <Footer />
        </>
    );
}

// Mount the App to the real DOM
const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(<App />);
```

### Component Tree
```
App
├── Header  (props: name)
├── Main    (props: user → {name, age, city})
└── Footer  (no props)
```

---

## 16. Important Interview Questions

### Q1: What is JSX?
> JSX (JavaScript XML) is a syntax extension for JavaScript that looks like HTML. It is transpiled by **Babel** into `React.createElement()` calls.

### Q2: What is the difference between React Element and React Component?
> A **React Element** is an immutable plain JS object describing a UI node. A **React Component** is a function or class that returns React elements (JSX). Components accept inputs (props) and can have logic.

### Q3: What is the Virtual DOM?
> Virtual DOM is a lightweight JS representation of the real DOM. When state changes, React creates a new Virtual DOM tree, **diffs** it with the previous one, and only updates the changed parts in the real DOM. This makes React fast.

### Q4: Why do we use `className` instead of `class` in JSX?
> `class` is a **reserved keyword** in JavaScript. JSX is JavaScript, so React uses `className` to avoid conflict.

### Q5: What are props?
> Props are read-only inputs passed from parent to child components. They allow data to flow **down** the component tree (unidirectional data flow).

### Q6: Why should list items have a `key` prop?
> Keys help React identify which items have **changed, added, or removed** in a list. Without keys, React re-renders the entire list, which is inefficient.

### Q7: What does `ReactDOM.createRoot()` do?
> It creates a React root attached to a DOM element. React renders and manages all content inside this root. It was introduced in **React 18** to enable **Concurrent Mode**.

### Q8: What is Babel's role in React?
> Babel is a JavaScript transpiler that converts **JSX and modern JS (ES6+)** into browser-compatible JavaScript. Without it, browsers cannot understand JSX.

### Q9: Can you render an Object directly in JSX?
> **No.** Rendering a plain JavaScript object in JSX throws an error: *"Objects are not valid as a React child."* You must access individual properties.

### Q10: What is a Fragment in React?
> A Fragment (`<>...</>` or `<React.Fragment>`) lets you return multiple elements from a component without adding extra DOM nodes like a `<div>`.

---

> 💡 **Tip for Placement:** Make sure you can draw the **JSX → createElement → React Element → DOM** flow on a whiteboard. It comes up very often in interviews!