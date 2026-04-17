---
layout: default
title: JavaScript
nav_order: 2
---

# 📜 JavaScript Interview Questions

Comprehensive notes on JavaScript fundamentals, advanced concepts, and modern ES6+ features.

---

## 📑 Table of Contents
- [1️⃣ var, let, and const](#1️⃣-var-let-and-const)
- [2️⃣ Closures](#2️⃣-closures)
- [3️⃣ == vs ===](#3️⃣--vs-)
- [4️⃣ What is `this`?](#4️⃣-what-is-this)
- [5️⃣ Debouncing vs Throttling](#5️⃣-debouncing-vs-throttling)
- [6️⃣ Call, Apply, and Bind](#6️⃣-call-apply-and-bind)
- [7️⃣ Shallow Clone vs Deep Clone](#7️⃣-shallow-clone-vs-deep-clone)
- [8️⃣ Hoisting](#8️⃣-hoisting)
- [9️⃣ Prototypal Inheritance](#9️⃣-prototypal-inheritance)
- [🔟 Blocking vs Non-Blocking](#🔟-blocking-vs-non-blocking)
- [1️⃣1️⃣ Currying](#1️⃣1️⃣-currying)
- [1️⃣2️⃣ Higher-Order Functions (HOF)](#1️⃣2️⃣-higher-order-functions-hof)
- [1️⃣3️⃣ Promises](#1️⃣3️⃣-promises)
- [1️⃣4️⃣ Memoization](#1️⃣4️⃣-memoization)
- [1️⃣5️⃣ Tree Shaking](#1️⃣5️⃣-tree-shaking)
- [1️⃣6️⃣ Connection Pooling](#1️⃣6️⃣-connection-pooling)
- [1️⃣7️⃣ Bundlers vs Transpilers](#1️⃣7️⃣-bundlers-vs-transpilers)
- [1️⃣8️⃣ JWKS (JSON Web Key Set)](#1️⃣8️⃣-jwks-json-web-key-set)
- [1️⃣9️⃣ Event Delegation](#1️⃣9️⃣-event-delegation)
- [2️⃣0️⃣ null vs undefined](#2️⃣0️⃣-null-vs-undefined)

---

## **1️⃣ var, let, and const**

### **`var` (Old Way – Avoid in Modern Code)**
* **Scope:** Function scoped
* **Can be re-declared:** Yes
* **Can be reassigned:** Yes

### **`let` (Modern Standard)**
* **Scope:** Block scoped
* **Can be reassigned:** Yes
* **Can be re-declared:** No (in same scope)
* **Hoisting:** Hoisted but in Temporal Dead Zone (TDZ)

### **`const` (Constant Variable)**
* **Scope:** Block scoped
* **Can be reassigned:** No
* **Can be re-declared:** No
* **Initialization:** Must be initialized at declaration

> [!NOTE]
> **Temporal Dead Zone (TDZ):** The time between entering a block scope and the execution of the variable’s declaration where the variable exists but cannot be accessed.

---

## **2️⃣ Closures**

A closure is when a function remembers variables from its outer scope even after the outer function has finished executing.

### **Example**
```javascript
function outer() {
  let count = 0;

  return function inner() {
    count++;
    console.log(count);
  };
}

const fn = outer();
fn(); // 1
fn(); // 2
```

**Why does this work?** Because `inner()` forms a closure over `count`.

---

## **3️⃣ == vs ===**

* **`==`** → Performs **type coercion** before comparison.
* **`===`** → Performs **strict comparison** (checks both value and type).

```javascript
5 == "5"   // true
5 === "5"  // false
```

> [!TIP]
> **Interview Gold:** Always prefer `===` to avoid unexpected bugs caused by type coercion.

---

## **4️⃣ What is `this`?**

In JavaScript, the `this` keyword refers to the context in which a function is executed. It points to the object that is currently calling the function.

1. **Global Context:** In global scope, `this` refers to the global object (`window` in browsers, `global` in Node.js).
2. **Object Method:** Inside a method, `this` refers to the object itself.
3. **Constructor Functions:** Refers to the newly created object.
4. **Arrow Functions:** Do NOT have their own `this`. They inherit it from the surrounding lexical context.

### **Example**
```javascript
const person = {
  name: "Alice",
  greet: function () {
    console.log(this.name);
  }
};

person.greet(); // Alice
```

---

## **5️⃣ Debouncing vs Throttling**

Both are used to **control how often a function runs** when an event happens many times.

### **1. Debouncing**
👉 **Waits until the user stops doing something.** The function runs only after the event stops for a specific time.
* **Example:** Search bar typing. User types `A → An → Ani`... Debounce makes only **1 API call** after the user stops.

```javascript
function debounce(fn, delay) {
  let timer;
  return function (...args) {
    clearTimeout(timer);
    timer = setTimeout(() => {
      fn(...args);
    }, delay);
  };
}
```

### **2. Throttling**
👉 **Ensures a function runs only once in a fixed time interval.**
* **Example:** Scrolling. If a user scrolls 100 times in 1s, throttling ensures the function runs only once every 300ms.

```javascript
function throttle(fn, delay) {
  let lastCall = 0;
  return function (...args) {
    const now = Date.now();
    if (now - lastCall >= delay) {
      lastCall = now;
      fn(...args);
    }
  };
}
```

---

## **6️⃣ Call, Apply, and Bind**

Used to control what `this` refers to.

### **1. call()**
Executes immediately. Arguments are passed **separated by commas**.
```javascript
greet.call(person1, 25, "Delhi");
```

### **2. apply()**
Executes immediately. Arguments are passed as an **array**.
```javascript
greet.apply(person2, [30, "Mumbai"]);
```

### **3. bind()**
Does **NOT** run immediately. Returns a **new function** with `this` fixed.
```javascript
const greetRahul = greet.bind(person2, 28, "Pune");
greetRahul();
```

---

## **7️⃣ Shallow Clone vs Deep Clone**

### **1. Shallow Clone**
Copies only the **first level**. Nested objects share the **same reference**.
* **Method:** `{ ...obj }` or `Object.assign({}, obj)`.

### **2. Deep Clone**
Copies **all levels**. Nested objects are fully independent.
* **Modern Way:** `structuredClone(obj)`
* **Old Way:** `JSON.parse(JSON.stringify(obj))`

> [!IMPORTANT]
> **Shallow Clone:** Changing a nested object in the clone *will* change the original.
> **Deep Clone:** Changing any part of the clone will *not* affect the original.

---

## **8️⃣ Hoisting**

JavaScript moves declarations to the top of their scope before execution.

1. **Function Declarations:** Fully hoisted. Can be called before definition.
2. **`var`:** Hoisted but initialized as `undefined`.
3. **`let` and `const`:** Hoisted but stay in **Temporal Dead Zone (TDZ)**. Accessing them early causes a `ReferenceError`.
4. **Function Expressions:** NOT hoisted as functions (hoisted as variables if using `var`).

> [!TIP]
> **Interview Gold:** Hoisting is the process of moving declarations to the top during the compilation phase.

---

## **9️⃣ Prototypal Inheritance**

Objects share properties and methods through a **prototype chain**. Every object has a prototype from which it inherits.

```javascript
const animal = {
  eats: true,
  walk() { console.log('Animal walks'); }
};

const dog = Object.create(animal);
console.log(dog.eats); // true (inherited)
```

---

## **🔟 Blocking vs Non-Blocking**

* **Blocking (Synchronous):** Execution stops until the operation finishes (e.g., `readFileSync`).
* **Non-Blocking (Asynchronous):** Execution continues; a callback/promise handles the result (e.g., `readFile`).

---

## **1️⃣1️⃣ Currying**

Converting a function with multiple arguments into a series of functions that each take **one argument**.

```javascript
const add = (a) => (b) => (c) => a + b + c;
add(2)(3)(4); // 9
```

**Practical Use:** Creating specialized logging functions (e.g., `const errorLog = log("ERROR")`).

---

## **1️⃣2️⃣ Higher-Order Functions (HOF)**

A function that:
1. Takes another function as an argument.
2. OR returns another function.

---

## **1️⃣3️⃣ Promises**

Represents the eventual completion or failure of an async operation.
* **States:** `pending`, `fulfilled`, `rejected`.

### **Handling Multiple Promises:**
1. **`Promise.all()`:** Resolves when **all** resolve. Rejects if **any** reject.
2. **`Promise.race()`:** Settles as soon as the **first** one settles.
3. **`Promise.any()`:** Resolves with the **first fulfilled** promise.
4. **`Promise.allSettled()`:** Waits for **all** to settle (regardless of success/failure).

---

## **1️⃣4️⃣ Memoization**

An optimization technique that stores function results to avoid expensive recalculations for the same inputs.

---

## **1️⃣5️⃣ Tree Shaking**

Removing unused code from the final bundle during the build process to reduce size.

---

## **1️⃣6️⃣ Connection Pooling**

Reusing existing database connections instead of creating new ones for every query to improve performance.

---

## **1️⃣7️⃣ Bundlers vs Transpilers**

* **Transpiler:** Converts modern syntax to older syntax for compatibility (e.g., Babel).
* **Bundler:** Combines multiple files into one optimized file (e.g., Webpack, Vite).

---

## **1️⃣8️⃣ JWKS (JSON Web Key Set)**

A JSON file containing public keys used to verify JWT tokens in authentication systems.

---

## **1️⃣9️⃣ Event Delegation**

Adding one event listener to a parent instead of many to children, utilizing **event bubbling**.

---

## **2️⃣0️⃣ null vs undefined**

* **`undefined`:** Declared but not assigned.
* **`null`:** Intentional assignment of "no value".
