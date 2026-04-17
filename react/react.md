---
layout: default
title: React
nav_order: 4
---

# ⚛️ React Interview Questions

From core fundamentals to advanced patterns and the latest features in React 19.

---

## 📑 Table of Contents
- [Core Fundamentals](#-core-fundamentals)
- [Hooks API](#-hooks-api)
- [Performance Optimization](#-performance-optimization)
- [Lifecycle & Rendering](#-lifecycle--rendering)
- [Advanced Concepts](#-advanced-concepts)
- [React 19 Features](#-react-19-features)

---

## 🏗️ Core Fundamentals

### ⚛️ Virtual DOM
A lightweight representation of the real DOM. React uses it to calculate the minimum number of changes needed before updating the browser's DOM.
* **Benefit:** Significantly faster UI updates.

### 🧩 Components
Reusable, independent pieces of UI. They can be Functional (modern standard) or Class-based (legacy).

---

## ⚓ Hooks API

| Hook | Purpose |
| :--- | :--- |
| **`useState`** | Manages local state within a component. |
| **`useEffect`** | Handles side effects (API calls, timers, event listeners). |
| **`useMemo`** | Memoizes a **calculated value** to prevent expensive re-computations. |
| **`useCallback`** | Memoizes a **function definition** to prevent unnecessary re-renders of children. |
| **`useRef`** | Holds a mutable value that doesn't trigger a re-render or accesses a DOM element directly. |

---

## ⚡ Performance Optimization

### 1️⃣ React.memo
A higher-order component that wraps a functional component to prevent it from re-rendering if its props haven't changed.

### 2️⃣ Lazy Loading
Loading components only when they are needed using `React.lazy()` and `Suspense`.
* **Benefit:** Reduces initial bundle size and speeds up page load.

### 3️⃣ Key Usage
Properly using the `key` prop in lists allows React to identify which items have changed, been added, or removed, ensuring efficient updates.

---

## 🔄 Lifecycle & Rendering

### The Three Phases:
1. **Mounting:** Component is created and inserted into the DOM.
2. **Updating:** Component re-renders due to changes in state, props, or parent updates.
3. **Unmounting:** Component is removed from the DOM (useful for cleanup).

> [!NOTE]
> **Hydration:** The process where React attaches event listeners to server-rendered HTML, making it interactive.

---

## 🧠 Advanced Concepts

### 1️⃣ Lifting State Up
Moving shared state to the closest common ancestor of components that need to access it.

### 2️⃣ Prop Drilling
The problem of passing data through multiple levels of components. 
* **Solutions:** Context API, Redux, or Zustand.

### 3️⃣ Reconciliation
The "diffing" algorithm React uses to compare the Virtual DOM with the previous version to update only the changed parts of the real DOM.

---

## 🚀 React 19 Features

### 1️⃣ `use()`
Allows reading async resources (like Promises) or Context directly in render.

### 2️⃣ Server Actions
Seamlessly handle form submissions and data mutations with functions that run on the server.

### 3️⃣ New Hooks for Forms:
* **`useFormStatus`:** Access pending state of a form submission.
* **`useActionState`:** Manage form state and async actions.
* **`useOptimistic`:** Implement instant UI updates while waiting for server confirmation.

> [!TIP]
> **Interview Gold:** React 19 focuses on simplifying async data handling and reducing the boilerplate needed for form management.
