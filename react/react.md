---
layout: default
title: React
nav_order: 4
---

# ⚛️ React Interview Questions

From core fundamentals to advanced patterns and the latest features in React 19.

---

## 📑 Table of Contents
- [1️⃣ Core Fundamentals](#1️⃣-core-fundamentals)
- [2️⃣ Hooks API](#2️⃣-hooks-api)
- [3️⃣ Rendering & Performance](#3️⃣-rendering--performance)
- [4️⃣ Controlled vs Uncontrolled Components](#4️⃣-controlled-vs-uncontrolled-components)
- [5️⃣ Important Concepts](#5️⃣-important-concepts)
- [6️⃣ Next.js Specific Questions](#6️⃣-nextjs-specific-questions)
- [7️⃣ React Lifecycle](#7️⃣-react-lifecycle)
- [8️⃣ Lazy Loading & Optimization](#8️⃣-lazy-loading--optimization)
- [9️⃣ Hydration](#9️⃣-hydration)
- [🔟 React 19 Features](#🔟-react-19-features)
- [1️⃣1️⃣ Shadow DOM vs Virtual DOM](#1️⃣1️⃣-shadow-dom-vs-virtual-dom)
- [1️⃣2️⃣ React Strict Mode](#1️⃣2️⃣-react-strict-mode)

---

## **1️⃣ Core Fundamentals**

### **What is React?**
React is a JavaScript library for building user interfaces using reusable components and a virtual DOM for efficient updates.

### **What is Virtual DOM?**
A lightweight copy of the real DOM. React compares the virtual DOM with a previous version (diffing) and updates only the changed parts in the real DOM (reconciliation).
* **Benefit:** Faster performance by minimizing expensive real DOM manipulations.

### **What is a Component?**
A reusable, independent piece of UI.

---

## **2️⃣ Hooks API**

### **`useState`**
Used to manage state in functional components.

**Simple Explanation:** State is the component's memory — data that it remembers between renders. When state changes, React re-renders the component to show the new value.

```javascript
import { useState } from "react";

function Counter() {
  const [count, setCount] = useState(0); // initial value = 0

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>+1</button>
      <button onClick={() => setCount(0)}>Reset</button>
    </div>
  );
}
```

> [!NOTE]
> **Never mutate state directly.** `count++` won't trigger a re-render. Always use the setter: `setCount(count + 1)`.

### **`useEffect`**
Used for side effects like API calls, event listeners, and timers.

**Simple Explanation:** A "side effect" is anything that reaches outside the component — fetching data, updating the document title, setting up a timer. `useEffect` runs *after* React has painted the screen, so it never blocks the UI.

```javascript
import { useState, useEffect } from "react";

function UserProfile({ userId }) {
  const [user, setUser] = useState(null);

  useEffect(() => {
    // Runs after render, whenever userId changes
    fetch(`/api/users/${userId}`)
      .then(res => res.json())
      .then(setUser);

    // Cleanup — runs before next effect or unmount
    return () => setUser(null);
  }, [userId]); // dependency array — re-run when userId changes

  if (!user) return <p>Loading...</p>;
  return <p>Hello, {user.name}</p>;
}
```

**Dependency array rules:**
* `[]` — run once on mount only
* `[value]` — run on mount + whenever `value` changes
* *(no array)* — run after every render (usually not what you want)

### **`useMemo`**
Memoizes a **calculated value** to prevent expensive re-computations on every render.
```javascript
const result = useMemo(() => expensiveCalc(data), [data]);
```

### **`useCallback`**
Memoizes a **function definition** to prevent it from being recreated on every render.
```javascript
const handleClick = useCallback(() => {
  console.log("clicked");
}, []);
```

### **`useRef`**
Holds a mutable value that doesn't trigger a re-render or accesses a DOM element directly.

**Simple Explanation:** Think of `useRef` as a sticky note attached to the component. You can write anything on it and it persists between renders — but changing it doesn't cause a re-render. Most common use: grabbing a DOM element directly.

```javascript
import { useRef } from "react";

function SearchInput() {
  const inputRef = useRef(null); // ref.current will point to the <input> DOM node

  function focusInput() {
    inputRef.current.focus(); // direct DOM access — no re-render needed
  }

  return (
    <div>
      <input ref={inputRef} type="text" placeholder="Search..." />
      <button onClick={focusInput}>Focus</button>
    </div>
  );
}
```

**`useRef` vs `useState`:**
* Use `useState` when the value should show in the UI (re-render on change).
* Use `useRef` when you need to track a value (timer ID, previous value, DOM node) but don't want it to trigger a re-render.

---

## **3️⃣ Rendering & Performance**

### **When does a component re-render?**
1. State changes.
2. Props change.
3. Parent component re-renders.

### **`React.memo`**
A higher-order component that prevents unnecessary re-renders of a functional component if its props haven't changed.

```javascript
// Without memo — re-renders every time parent re-renders, even if name didn't change
function Greeting({ name }) {
  console.log("Greeting rendered");
  return <h1>Hello, {name}</h1>;
}

// With memo — only re-renders if name prop actually changes
const Greeting = React.memo(function({ name }) {
  console.log("Greeting rendered");
  return <h1>Hello, {name}</h1>;
});
```

> [!NOTE]
> **When NOT to use `React.memo`:** Don't wrap every component. Memo has its own overhead (comparison cost). Only use it when a component renders often and its props rarely change (e.g., items in a large list).

### **Optimization Strategy**
> [!TIP]
> **Interview Gold:** To optimize a slow React app, first use the **React DevTools Profiler** to find bottlenecks. Then use `React.memo`, `useMemo`, and `useCallback` strategically. Implement lazy loading, virtualize large lists (e.g., `react-window`), and ensure stable keys in lists.

---

## **4️⃣ Controlled vs Uncontrolled Components**

* **Controlled:** React handles the state of the input.
  ```javascript
  <input value={name} onChange={e => setName(e.target.value)} />
  ```
* **Uncontrolled:** The DOM handles the input state; accessed via `refs`.
  ```javascript
  <input ref={inputRef} />
  ```

---

## **5️⃣ Important Concepts**

### **Lifting State Up**
Moving shared state to the closest common ancestor to allow multiple components to access it.

**Simple Explanation:** When two sibling components need to share the same piece of data, move that state "up" to their parent. The parent holds the state and passes it down as props.

```javascript
// ❌ Before: each component has its own count — they don't stay in sync
function ComponentA() { const [count, setCount] = useState(0); ... }
function ComponentB() { const [count, setCount] = useState(0); ... }

// ✅ After: parent holds count, both children read from the same source
function Parent() {
  const [count, setCount] = useState(0); // lifted up here

  return (
    <>
      <ComponentA count={count} />
      <ComponentB count={count} onIncrement={() => setCount(c => c + 1)} />
    </>
  );
}
```

### **Keys in React**
Keys help React identify which items have changed, been added, or removed. Avoid using indexes as keys if the list order can change.

**Why index keys break re-ordering:**
```javascript
// ❌ Index keys — if you sort/delete, React reuses wrong DOM nodes
{items.map((item, index) => <li key={index}>{item.name}</li>)}

// ✅ Stable unique ID — React always matches the right element
{items.map(item => <li key={item.id}>{item.name}</li>)}
```

> [!NOTE]
> Index keys are fine ONLY for **static, non-reorderable** lists (like tabs that never change order).

### **Reconciliation**
The "diffing" algorithm React uses to update only the changed parts of the real DOM.

**Simple Explanation:** React is like a painter who takes a photo of your room (Virtual DOM snapshot), then when something changes, takes a new photo and compares the two. Only the things that are *different* get repainted — not the whole room. This is much faster than repainting everything from scratch on every state change.

### **Prop Drilling**
The problem of passing data through many levels of components.
* **Solutions:** Context API, Zustand, Redux.

**What it looks like:**
```javascript
// ❌ Drilling `user` through 3 levels just to use it in Avatar
function App() {
  const user = { name: "Alice" };
  return <Layout user={user} />;
}
function Layout({ user }) {
  return <Header user={user} />; // Layout doesn't use user, just passes it
}
function Header({ user }) {
  return <Avatar user={user} />; // Header doesn't use user, just passes it
}
function Avatar({ user }) {
  return <img src={user.avatarUrl} />; // finally used here
}

// ✅ Fix: Context API — Avatar reads directly, no drilling
const UserContext = createContext();
function App() {
  return <UserContext.Provider value={user}><Layout /></UserContext.Provider>;
}
function Avatar() {
  const user = useContext(UserContext); // grabs directly
  return <img src={user.avatarUrl} />;
}
```

---

## **6️⃣ Next.js Specific Questions**

### **CSR vs SSR**
* **CSR (Client Side Rendering):** Browser downloads JS and renders the UI.
* **SSR (Server Side Rendering):** Server generates HTML on each request.
* **Next.js:** Supports CSR, SSR, SSG (Static Site Generation), and ISR (Incremental Static Regeneration).

---

## **7️⃣ React Lifecycle**

### **1. Mounting (Birth)**
Component appears on screen.
```javascript
useEffect(() => {
  console.log("Component mounted");
}, []); // Empty dependency array
```

### **2. Updating (Growth)**
Component re-renders due to state/prop changes.
```javascript
useEffect(() => {
  console.log("Count changed");
}, [count]);
```

### **3. Unmounting (Death)**
Component is removed from screen.
```javascript
useEffect(() => {
  return () => {
    console.log("Cleanup: Component unmounted");
  };
}, []);
```

---

## **8️⃣ Lazy Loading & Optimization**

### **Lazy Loading (React.lazy)**
Load components only when needed to reduce initial bundle size.
```javascript
const Dashboard = React.lazy(() => import("./Dashboard"));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Dashboard />
    </Suspense>
  );
}
```

### **Tree Shaking**
Removing unused branches (functions/code) from the final bundle to improve performance.

---

## **9️⃣ Hydration**

The process where React "activates" server-rendered HTML on the client by attaching event listeners.
* **Flow:** Server sends static HTML → Browser displays it (fast paint) → React hydrates the HTML → Page becomes interactive.

---

## **🔟 React 19 Features**

### **`use()`**
Allows reading async resources (like Promises) or Context directly during rendering.
```javascript
const data = use(fetchData());
```

### **`useFormStatus()`**
Accesses submission status (e.g., `pending`) of a form, especially with Server Actions.

```javascript
"use client";
import { useFormStatus } from "react-dom";

function SubmitButton() {
  const { pending } = useFormStatus(); // reads the form's submission state
  return <button disabled={pending}>{pending ? "Saving..." : "Submit"}</button>;
}
```

### **`useActionState()`**
A cleaner way to manage form/action state (errors, loading, data).
```javascript
const [state, formAction, isPending] = useActionState(createUser, null);
```

**What each value is:**
* `state` — the current state (errors or success data returned from the action)
* `formAction` — pass this as the form's `action` prop
* `isPending` — `true` while the action is running

### **`useOptimistic()`**
Implements instant UI updates before server confirmation (e.g., chat messages or like buttons).

```javascript
"use client";
import { useOptimistic } from "react";

function LikeButton({ post }) {
  const [optimisticLikes, addOptimisticLike] = useOptimistic(
    post.likes,
    (current, increment) => current + increment
  );

  async function handleLike() {
    addOptimisticLike(1);           // immediately show +1 in UI
    await likePost(post.id);        // actual server call in background
    // if server fails, React automatically reverts to real value
  }

  return <button onClick={handleLike}>❤️ {optimisticLikes}</button>;
}
```

---

## **1️⃣1️⃣ Shadow DOM vs Virtual DOM**

* **Virtual DOM:** A React-specific concept (JS object) used for performance optimization.
* **Shadow DOM:** A native browser technology used for scoping CSS and HTML (Web Components).

**As a React developer, you'll almost never touch Shadow DOM.** It's used internally by the browser for elements like `<video>` controls and `<input type="date">`. Web Components use it to scope their CSS so it doesn't leak. React uses Virtual DOM internally for its own diffing — completely separate concept, despite the similar name.

---

## **1️⃣2️⃣ React Strict Mode**

In development, React 18+ intentionally mounts/unmounts/remounts components to help identify side-effect bugs and ensure cleanup functions are properly implemented.

**Why it double-invokes:** React simulates "mount → unmount → remount" to catch bugs where your `useEffect` cleanup doesn't properly undo what the setup did.

**Bug it catches:**
```javascript
// ❌ This leaks a setInterval in Strict Mode
useEffect(() => {
  const id = setInterval(() => console.log("tick"), 1000);
  // Forgot the cleanup! When component unmounts, interval keeps running
}, []);

// ✅ Correct — cleanup returns the teardown function
useEffect(() => {
  const id = setInterval(() => console.log("tick"), 1000);
  return () => clearInterval(id); // Strict Mode will test this runs correctly
}, []);
```

> [!NOTE]
> **Strict Mode only runs in development.** It doesn't affect your production build. If your app behaves differently in development vs production, you likely have a missing cleanup in a `useEffect`.

---

## **1️⃣3️⃣ Context API**

Context lets you share data across the component tree without passing props at every level (prop drilling solution).

### **How it works**
1. Create a context with `createContext()`.
2. Wrap the part of your tree that needs the data in a `Provider`.
3. Any child component can read the value using `useContext()`.

```javascript
// 1. Create
const ThemeContext = createContext("light");

// 2. Provide
function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Toolbar />
    </ThemeContext.Provider>
  );
}

// 3. Consume (anywhere inside the Provider)
function Toolbar() {
  const theme = useContext(ThemeContext);
  return <div>Current theme: {theme}</div>;
}
```

> [!TIP]
> **Interview Gold:** Context is not a state management library — it's a data transport mechanism. Every component that consumes the context **re-renders when the value changes**. For frequently-changing global state, use Zustand or Redux instead.

---

## **1️⃣4️⃣ Error Boundaries**

An Error Boundary is a class component that catches JavaScript errors in its child tree and shows a fallback UI instead of crashing the whole page.

* Only catches errors during **rendering**, not in event handlers or async code.
* Must be a **class component** (no hook equivalent yet).

```javascript
class ErrorBoundary extends React.Component {
  state = { hasError: false };

  static getDerivedStateFromError(error) {
    return { hasError: true };
  }

  componentDidCatch(error, info) {
    console.error("Error caught:", error, info);
  }

  render() {
    if (this.state.hasError) return <h1>Something went wrong.</h1>;
    return this.props.children;
  }
}

// Usage
<ErrorBoundary>
  <MyComponent />
</ErrorBoundary>
```

> [!TIP]
> **Interview Gold:** Wrap each major section of your UI (sidebar, feed, widget) in its own Error Boundary so one broken component doesn't take down the whole app.

---

## **1️⃣5️⃣ Custom Hooks**

A custom hook is a JavaScript function whose name starts with `use` that calls other hooks. It lets you **extract and reuse stateful logic** between components.

```javascript
// Custom hook
function useFetch(url) {
  const [data, setData] = useState(null);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(data => {
        setData(data);
        setLoading(false);
      });
  }, [url]);

  return { data, loading };
}

// Use it in any component
function UserList() {
  const { data, loading } = useFetch("/api/users");
  if (loading) return <p>Loading...</p>;
  return <ul>{data.map(u => <li key={u.id}>{u.name}</li>)}</ul>;
}
```

> [!TIP]
> **Interview Gold:** Custom hooks don't share state — each component gets its own isolated state. They only share the **logic**. This is the React way of doing what mixins did in older patterns.

---

## **1️⃣6️⃣ React Portals**

Portals let you render a child component **outside the parent DOM hierarchy**, while still keeping it in the React tree.

* Use case: Modals, tooltips, dropdowns — things that need to visually "escape" a parent with `overflow: hidden` or a low z-index.

```javascript
import { createPortal } from "react-dom";

function Modal({ children }) {
  return createPortal(
    <div className="modal">{children}</div>,
    document.getElementById("modal-root") // render here in the real DOM
  );
}
```

* Events still bubble through the React tree normally, even though the DOM placement is different.

> [!TIP]
> **Interview Gold:** Portals solve the classic `z-index` and `overflow: hidden` problems with modals. The modal renders at the `<body>` level in the real DOM but behaves like it's inside your component in React's event system.

---

## **1️⃣7️⃣ State Management: Zustand vs Redux**

Both solve the same problem — global state shared across many components — but in very different ways.

### **Redux**
* Strict, opinionated: actions → reducer → store.
* Lots of boilerplate (action types, reducers, selectors).
* Best for large teams and complex state with many transitions.
* Redux Toolkit (RTK) modernizes it with less boilerplate.

### **Zustand**
* Minimal, flexible: just a store with functions.
* Very little boilerplate — set up in minutes.
* Best for small to medium apps where you want simplicity.

```javascript
// Zustand — entire store in ~10 lines
import { create } from "zustand";

const useCountStore = create((set) => ({
  count: 0,
  increment: () => set((state) => ({ count: state.count + 1 })),
}));

// In a component
const { count, increment } = useCountStore();
```

> [!TIP]
> **Interview Gold:** For new projects, start with **Zustand** — it's simpler and you can always migrate later. Choose **Redux** when you need strict action history, time-travel debugging, or your team is already using it.
