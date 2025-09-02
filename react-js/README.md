# React Interview Questions & Answers

## Table of Contents
1. [What is JSX in React, and how does it differ from HTML?](#1-what-is-jsx-in-react-and-how-does-it-differ-from-html-provide-an-example-of-how-you-would-use-jsx-to-render-a-dynamic-list-of-items)
2. [Explain the difference between functional and class components in React.](#2-explain-the-difference-between-functional-and-class-components-in-react-when-would-you-prefer-one-over-the-other)
3. [What are props in React?](#3-what-are-props-in-react-how-do-they-differ-from-state-and-can-you-give-an-example-of-prop-drilling-and-how-to-avoid-it)
4. [Describe React state management.](#4-describe-react-state-management-provide-an-example-using-the-usestate-hook-and-explain-when-to-lift-state-up)
5. [How does event handling work in React?](#5-how-does-event-handling-work-in-react-give-an-example-of-handling-a-form-submission-with-controlled-components)
6. [What is conditional rendering in React?](#6-what-is-conditional-rendering-in-react-provide-examples-using-ternary-operators-and-logical-and)
7. [Explain lists and keys in React.](#7-explain-lists-and-keys-in-react-why-are-keys-important-and-what-happens-if-theyre-missing)
8. [What are React hooks?](#8-what-are-react-hooks-explain-usestate-and-useeffect-with-examples)
9. [Describe the Context API in React.](#9-describe-the-context-api-in-react-when-would-you-use-it-over-prop-drilling)
10. [What is React Router?](#10-what-is-react-router-provide-an-example-of-setting-up-basic-routing-with-navigation)
11. [Explain state management with Redux.](#11-explain-state-management-with-redux-what-are-actions-reducers-and-the-store)
12. [How do you handle side effects and data fetching in React?](#12-how-do-you-handle-side-effects-and-data-fetching-in-react-give-an-example-with-useeffect-and-fetch)
13. [What are error boundaries in React?](#13-what-are-error-boundaries-in-react-provide-an-example-implementation)
14. [Explain refs in React.](#14-explain-refs-in-react-when-would-you-use-forwarding-refs)
15. [What is the difference between composition and inheritance in React?](#15-what-is-the-difference-between-composition-and-inheritance-in-react-why-is-composition-preferred)

---

## 1. What is JSX in React, and how does it differ from HTML? Provide an example of how you would use JSX to render a dynamic list of items.

JSX (JavaScript XML) is a syntax extension that allows developers to write HTML-like code within JavaScript, which React transpiles into JavaScript objects using tools like Babel. It differs from HTML in that it's not a string but JavaScript expressions; for instance, attributes use camelCase (e.g., `className` instead of `class`), and dynamic values are embedded with curly braces `{}`. JSX enables declarative UI descriptions and integrates seamlessly with JavaScript logic.

**Example:**
```jsx
function ItemList({ items }) {
  return (
    <ul>
      {items.map((item) => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
}
```
This demonstrates list rendering, where `map` creates dynamic `<li>` elements, and the `key` prop optimizes React's reconciliation process.

---

## 2. Explain the difference between functional and class components in React. When would you prefer one over the other?

Functional components are plain JavaScript functions that return JSX and are stateless by default, but can manage state with hooks like `useState`. Class components extend `React.Component`, use `this` for state and props, and have lifecycle methods like `componentDidMount`. Functional components are preferred in modern React (since hooks in v16.8) for their simplicity, less boilerplate, and better performance. Use class components for legacy code or when needing error boundaries (which require `componentDidCatch`).

**Example of functional:**
```jsx
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```
Prefer functional for new code; class for maintaining older projects.

---

## 3. What are props in React? How do they differ from state, and can you give an example of prop drilling and how to avoid it?

Props (properties) are read-only data passed from parent to child components, enabling reusability and data flow. State is mutable data managed within a component using `useState` or `this.state`, triggering re-renders on updates. Props are external inputs; state is internal. Prop drilling occurs when props are passed through multiple nested components, leading to cluttered code.

**Example of prop drilling:**
```jsx
function Grandparent() {
  return <Parent data="value" />;
}
function Parent({ data }) {
  return <Child data={data} />;
}
function Child({ data }) {
  return <p>{data}</p>;
}
```

**Avoid it with Context API:**
```jsx
const DataContext = createContext();
function Grandparent() {
  return (
    <DataContext.Provider value="value">
      <Child />
    </DataContext.Provider>
  );
}
function Child() {
  const data = useContext(DataContext);
  return <p>{data}</p>;
}
```

---

## 4. Describe React state management. Provide an example using the useState hook and explain when to lift state up.

State in React holds dynamic data that can change over time, managed locally in components. The `useState` hook initializes and updates state in functional components, returning a value and setter function. Lift state up when multiple components need shared access, moving it to the nearest common ancestor.

**Example:**
```jsx
function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>{count}</button>;
}
```
Lift state up example: Move `count` to a parent if siblings need to display and update it, passing via props.

---

## 5. How does event handling work in React? Give an example of handling a form submission with controlled components.

React uses synthetic events for cross-browser consistency. Event handlers are attached via props like `onClick` and receive an event object. Controlled components tie input values to state, updating via `onChange`.

**Example:**
```jsx
function Form() {
  const [value, setValue] = useState('');
  const handleSubmit = (e) => {
    e.preventDefault();
    alert(value);
  };
  return (
    <form onSubmit={handleSubmit}>
      <input value={value} onChange={(e) => setValue(e.target.value)} />
      <button type="submit">Submit</button>
    </form>
  );
}
```
This prevents default submission and handles state-controlled input.

---

## 6. What is conditional rendering in React? Provide examples using ternary operators and logical AND.

Conditional rendering displays UI based on conditions, using JavaScript logic in JSX like if-else, ternaries, or `&&`. It's efficient for dynamic UIs without separate templates.

**Examples:**
- Ternary: `{isLoggedIn ? <p>Welcome</p> : <p>Login</p>}`
- Logical AND: `{loading && <p>Loading...</p>}`

**Full component:**
```jsx
function Greeting({ isLoggedIn }) {
  return isLoggedIn ? <p>Welcome</p> : <p>Please log in</p>;
}
```

---

## 7. Explain lists and keys in React. Why are keys important, and what happens if they're missing?

Lists render arrays of data using `map` to create elements. Keys are unique identifiers (strings) for each item, helping React track changes during reconciliation for efficient updates. Without keys, React uses indices, which can cause bugs like incorrect re-renders or state loss during reordering.

**Example:**
```jsx
function List({ items }) {
  return (
    <ul>
      {items.map((item) => <li key={item.id}>{item.name}</li>)}
    </ul>
  );
}
```
Use stable IDs (e.g., database IDs) as keys.

---

## 8. What are React hooks? Explain useState and useEffect with examples.

Hooks are functions for state and lifecycle features in functional components (introduced in v16.8). `useState` manages state; `useEffect` handles side effects like data fetching.

**Examples:**
- useState: 
```jsx
const [count, setCount] = useState(0);
```

- useEffect: 
```jsx
useEffect(() => {
  document.title = `Count: ${count}`;
  return () => document.title = 'React App'; // Cleanup
}, [count]);
```

Hooks follow rules: call at top level, not in loops/conditions.

---

## 9. Describe the Context API in React. When would you use it over prop drilling?

Context API shares data across components without props, using `createContext`, `Provider`, and `useContext`. It's for global state like themes or auth. Use over prop drilling for deeply nested components to avoid clutter.

**Example:**
```jsx
const ThemeContext = createContext('light');
function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Child />
    </ThemeContext.Provider>
  );
}
function Child() {
  const theme = useContext(ThemeContext);
  return <p>Theme: {theme}</p>;
}
```

---

## 10. What is React Router? Provide an example of setting up basic routing with navigation.

React Router handles client-side routing in SPAs, mapping URLs to components. Key components: `BrowserRouter`, `Routes`, `Route`, `Link`. It's declarative and supports dynamic paths.

**Example (v6+):**
```jsx
import { BrowserRouter, Routes, Route, Link } from 'react-router-dom';
function App() {
  return (
    <BrowserRouter>
      <nav><Link to="/">Home</Link></nav>
      <Routes>
        <Route path="/" element={<Home />} />
      </Routes>
    </BrowserRouter>
  );
}
function Home() { return <h2>Home</h2>; }
```

---

## 11. Explain state management with Redux. What are actions, reducers, and the store?

Redux centralizes state in a store for predictability. Actions are objects describing changes (e.g., `{ type: 'INCREMENT' }`). Reducers are pure functions updating state based on actions. The store holds state and dispatches actions.

**Example with Toolkit:**
```jsx
import { configureStore, createSlice } from '@reduxjs/toolkit';
const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: { increment: (state) => { state.value += 1; } },
});
const store = configureStore({ reducer: { counter: counterSlice.reducer } });
```
Use for large apps; Context for simpler needs.

---

## 12. How do you handle side effects and data fetching in React? Give an example with useEffect and fetch.

Side effects (e.g., API calls) use `useEffect`. Data fetching typically involves `fetch` or `axios` inside `useEffect`, updating state on response.

**Example:**
```jsx
function Users() {
  const [users, setUsers] = useState([]);
  useEffect(() => {
    fetch('https://jsonplaceholder.typicode.com/users')
      .then((res) => res.json())
      .then(setUsers);
  }, []);
  return <ul>{users.map((u) => <li key={u.id}>{u.name}</li>)}</ul>;
}
```
Include cleanup if needed, like aborting fetches.

---

## 13. What are error boundaries in React? Provide an example implementation.

Error boundaries catch errors in child trees, displaying fallback UI via class components with `getDerivedStateFromError` and `componentDidCatch`.

**Example:**
```jsx
class ErrorBoundary extends React.Component {
  state = { hasError: false };
  static getDerivedStateFromError() { return { hasError: true }; }
  render() {
    return this.state.hasError ? <p>Error!</p> : this.props.children;
  }
}
function App() {
  return <ErrorBoundary><BuggyComponent /></ErrorBoundary>;
}
```
Use for graceful error handling.

---

## 14. Explain refs in React. When would you use forwarding refs?

Refs access DOM nodes or component instances imperatively. Use `useRef` for mutable values or DOM manipulation. Forwarding refs passes a ref from parent to child DOM using `forwardRef`.

**Example:**
```jsx
const Input = forwardRef((props, ref) => <input ref={ref} {...props} />);
function App() {
  const ref = useRef();
  useEffect(() => ref.current.focus(), []);
  return <Input ref={ref} />;
}
```
Use for focus management or integrations.

---

## 15. What is the difference between composition and inheritance in React? Why is composition preferred?

Composition builds UIs by combining components (e.g., via props/children). Inheritance extends classes for reuse, but creates rigid hierarchies. Composition is preferred for flexibility, reusability, and alignment with React's declarative nature.

**Example of composition:**
```jsx
function Card({ children }) {
  return <div>{children}</div>;
}
function App() {
  return <Card><p>Content</p></Card>;
}
```
Avoid inheritance; use hooks for logic reuse.

---
