---
layout: default
title: JavaScript
nav_order: 2
---

# 📜 JavaScript Interview Questions

A deep dive into JavaScript fundamentals, memory management, and advanced asynchronous patterns.

---

## 📑 Table of Contents
- [Variables & Scope (var, let, const)](#-variables--scope-var-let-const)
- [Closures](#-closures)
- [Comparison (== vs ===)](#-comparison--vs-)
- [The `this` Keyword](#-the-this-keyword)
- [Performance: Debouncing vs Throttling](#-performance-debouncing-vs-throttling)
- [Function Context: Call, Apply, Bind](#-function-context-call-apply-bind)
- [Object Cloning: Shallow vs Deep](#-object-cloning-shallow-vs-deep)
- [Hoisting](#-hoisting)
- [Prototypal Inheritance](#-prototypal-inheritance)
- [Asynchronous JavaScript: Promises](#-asynchronous-javascript-promises)
- [Optimization: Memoization & Tree Shaking](#-optimization-memoization--tree-shaking)

---

## 🏗️ Variables & Scope (var, let, const)

| Feature | `var` | `let` | `const` |
| :--- | :--- | :--- | :--- |
| **Scope** | Function Scoped | Block Scoped | Block Scoped |
| **Re-declaration** | Allowed | Not Allowed | Not Allowed |
| **Re-assignment** | Allowed | Allowed | Not Allowed |
| **Hoisting** | Initialized as `undefined` | Hoisted but in TDZ | Hoisted but in TDZ |

> [!NOTE]
> **Temporal Dead Zone (TDZ):** The period between entering a block scope and the variable's declaration where accessing it throws a `ReferenceError`.

---

## 🧠 Closures

A **closure** is created when a function is defined inside another function and retains access to the outer function's scope, even after the outer function has finished executing.

```javascript
function createCounter() {
  let count = 0;
  return function() {
    count++;
    return count;
  };
}
const counter = createCounter();
console.log(counter()); // 1
console.log(counter()); // 2
```

---

## 🎯 The `this` Keyword

The value of `this` depends on **how** a function is called:
1. **Global context:** `window` (browser) or `global` (Node.js).
2. **Object method:** Refers to the object owning the method.
3. **Constructor:** Refers to the newly created instance.
4. **Arrow functions:** Inherit `this` from the surrounding lexical scope (they don't have their own `this`).

---

## ⚡ Performance: Debouncing vs Throttling

### 1️⃣ Debouncing
Waits for the user to **stop** performing an action for a specific duration before triggering the function.
* *Use Case:* Search bar API calls.

### 2️⃣ Throttling
Ensures a function runs **at most once** in a fixed time interval, regardless of how many times the event triggers.
* *Use Case:* Scroll listeners or window resizing.

---

## 🔗 Function Context: Call, Apply, Bind

| Method | Execution | Arguments | Returns |
| :--- | :--- | :--- | :--- |
| **`call()`** | Immediate | Comma-separated | Result of function |
| **`apply()`** | Immediate | Array | Result of function |
| **`bind()`** | Deferred | Comma-separated | A new function |

---

## 🧬 Object Cloning: Shallow vs Deep

### 1️⃣ Shallow Clone
Copies the first level. Nested objects share the same memory reference.
* *Method:* `{...obj}` or `Object.assign()`.

### 2️⃣ Deep Clone
Copies all levels. Nested objects are completely independent.
* *Method:* `structuredClone(obj)` (Modern) or `JSON.parse(JSON.stringify(obj))`.

---

## 🚀 Hoisting

Hoisting is JavaScript's default behavior of moving declarations to the top of the current scope.
* **Functions:** Fully hoisted (can be called before definition).
* **`var`:** Hoisted but initialized as `undefined`.
* **`let`/`const`:** Hoisted but uninitialized (TDZ).

---

## 🏗️ Prototypal Inheritance

In JavaScript, objects inherit properties and methods from other objects via the **Prototype Chain**.
* If a property is not found on an object, JS looks up the chain until it reaches `Object.prototype`.

---

## ⏳ Asynchronous JavaScript: Promises

A **Promise** represents the eventual completion or failure of an asynchronous operation.

### Promise States:
* **Pending:** Initial state.
* **Fulfilled:** Success.
* **Rejected:** Failed.

### Handling Multiple Promises:
* **`Promise.all()`:** Wait for all to succeed. Rejects if any fail.
* **`Promise.race()`:** Settles as soon as the first promise (success or fail) settles.
* **`Promise.allSettled()`:** Waits for all to finish, regardless of success/fail.
* **`Promise.any()`:** Returns the first fulfilled promise.

---

## 🌳 Optimization: Memoization & Tree Shaking

### Memoization
An optimization technique that stores the results of expensive function calls and returns the cached result when the same inputs occur again.

### Tree Shaking
A form of dead-code elimination. Bundlers (Webpack, Vite) remove unused exports from the final bundle to reduce size.

> [!TIP]
> **Interview Gold:** To enable Tree Shaking, you must use **ES Modules** (`import/export`) instead of CommonJS (`require/module.exports`).
