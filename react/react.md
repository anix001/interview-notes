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

### **`useEffect`**
Used for side effects like API calls, event listeners, and timers.

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

---

## **3️⃣ Rendering & Performance**

### **When does a component re-render?**
1. State changes.
2. Props change.
3. Parent component re-renders.

### **`React.memo`**
A higher-order component that prevents unnecessary re-renders of a functional component if its props haven't changed.

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

### **Keys in React**
Keys help React identify which items have changed, been added, or removed. Avoid using indexes as keys if the list order can change.

### **Reconciliation**
The "diffing" algorithm React uses to update only the changed parts of the real DOM.

### **Prop Drilling**
The problem of passing data through many levels of components.
* **Solutions:** Context API, Zustand, Redux.

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

### **`useActionState()`**
A cleaner way to manage form/action state (errors, loading, data).
```javascript
const [state, formAction, isPending] = useActionState(createUser, null);
```

### **`useOptimistic()`**
Implements instant UI updates before server confirmation (e.g., chat messages or like buttons).

---

## **1️⃣1️⃣ Shadow DOM vs Virtual DOM**

* **Virtual DOM:** A React-specific concept (JS object) used for performance optimization.
* **Shadow DOM:** A native browser technology used for scoping CSS and HTML (Web Components).

---

## **1️⃣2️⃣ React Strict Mode**

In development, React 18+ intentionally mounts/unmounts/remounts components to help identify side-effect bugs and ensure cleanup functions are properly implemented.
