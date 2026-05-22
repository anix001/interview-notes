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

**TDZ in action:**
```javascript
console.log(x); // ✅ undefined  (var is hoisted and initialized)
console.log(y); // ❌ ReferenceError (let is in TDZ)

var x = 5;
let y = 10;
```

> [!TIP]
> **Interview Gold:** Use `const` by default. Use `let` only when you need to reassign. Never use `var` in modern code — its function scope and hoisting cause subtle bugs.

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

**Simple Explanation:** HOFs let you write reusable logic and pass behaviour as data. The built-in array methods `map`, `filter`, and `reduce` are the most common HOFs.

```javascript
const numbers = [1, 2, 3, 4, 5];

// map — transforms each element, returns new array
const doubled = numbers.map(n => n * 2);       // [2, 4, 6, 8, 10]

// filter — keeps only elements that pass the test
const evens = numbers.filter(n => n % 2 === 0); // [2, 4]

// reduce — collapses the array into a single value
const sum = numbers.reduce((acc, n) => acc + n, 0); // 15
```

> [!TIP]
> **Interview Gold:** `map` returns a new array of the same length. `filter` returns a shorter array. `reduce` returns anything — a number, object, or string. Knowing when to use which is a key signal of JS maturity.

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

**Simple Explanation:** Think of it like a student writing answers on a cheat sheet. The first time they solve a problem they write it down. Next time the same problem appears, they just read the answer instead of solving it again.

```javascript
function memoize(fn) {
  const cache = {};
  return function (n) {
    if (cache[n] !== undefined) {
      console.log("From cache!");
      return cache[n];        // return stored result
    }
    cache[n] = fn(n);         // compute and store
    return cache[n];
  };
}

function slowSquare(n) {
  // imagine this takes 1 second
  return n * n;
}

const fastSquare = memoize(slowSquare);
fastSquare(5); // computed
fastSquare(5); // "From cache!" — instant
```

> [!TIP]
> **Interview Gold:** Memoization trades **memory for speed**. Use it for pure functions (same input always gives same output) with expensive computations — like Fibonacci, factorial, or heavy API transforms. React's `useMemo` is memoization built into the framework.

---

## **1️⃣5️⃣ Tree Shaking**

Removing unused code from the final bundle during the build process to reduce size.

**Simple Explanation:** Imagine a tree where every function is a leaf. Tree shaking "shakes" the tree — only the leaves you actually use fall into your bundle. Unused code never ships to the browser.

```javascript
// utils.js — you export 3 functions
export function add(a, b) { return a + b; }
export function subtract(a, b) { return a - b; }
export function multiply(a, b) { return a * b; } // never imported anywhere

// main.js — you only import 2
import { add, subtract } from "./utils.js";

// After tree shaking: multiply() is removed from the final bundle
```

**Why ES modules?** Tree shaking only works with `import/export` (ES modules), not `require()` (CommonJS). This is because `import` is static (the bundler knows at build time what's used), while `require()` is dynamic (evaluated at runtime).

> [!TIP]
> **Interview Gold:** Tree shaking is done by bundlers like Webpack and Vite automatically. To benefit from it, always use named exports (`export function`) rather than exporting a whole object — the bundler can't shake individual keys off a default-exported object.

---

## **1️⃣6️⃣ Connection Pooling**

Reusing existing database connections instead of creating new ones for every query to improve performance.

**Simple Explanation:** Opening a database connection is like hiring a taxi — it takes time and costs resources. Connection pooling is like having a taxi rank: a set of taxis already running and waiting. When your app needs a DB query, it grabs an available taxi from the rank, uses it, then returns it. No hiring time, no waste.

**Without pooling** — every query opens a new connection:
```javascript
// ❌ Slow — creates a new TCP connection for every single query
const client = new Client(config);
await client.connect();          // ~50-100ms overhead per query
const result = await client.query("SELECT * FROM users");
await client.end();
```

**With pooling** — connections are reused:
```javascript
// ✅ Fast — connections are pre-created and reused
const { Pool } = require("pg");
const pool = new Pool({ max: 10 }); // keep up to 10 connections open

const result = await pool.query("SELECT * FROM users"); // grabs an idle connection
// connection is returned to pool automatically
```

> [!TIP]
> **Interview Gold:** Without connection pooling, a high-traffic Node.js app can exhaust the database's max-connection limit and crash. Most ORMs (Prisma, Sequelize) use pooling by default. In production, always set a sensible `max` pool size.

---

## **1️⃣7️⃣ Bundlers vs Transpilers**

* **Transpiler:** Converts modern syntax to older syntax for compatibility (e.g., Babel).
* **Bundler:** Combines multiple files into one optimized file (e.g., Webpack, Vite).

**Simple Explanation:** They solve different problems and are often used together.

| | Transpiler (Babel) | Bundler (Webpack / Vite) |
|---|---|---|
| **What it does** | Translates code | Packages code |
| **Input** | Modern JS / TypeScript | Many separate files |
| **Output** | Older JS browsers understand | One (or few) optimized files |

**Transpiler example** — Babel converts modern JS to older syntax:
```javascript
// Input (modern — arrow function, optional chaining)
const greet = (user) => `Hello ${user?.name}`;

// Output (Babel transpiled for older browsers)
var greet = function(user) {
  return "Hello " + (user === null || user === void 0 ? void 0 : user.name);
};
```

**Bundler example** — Vite/Webpack combines your 100 files into 1:
```
src/
  index.js  (imports App.js)
  App.js    (imports Button.js, utils.js)
  Button.js
  utils.js

→ bundle.js  (one file with everything)
```

> [!TIP]
> **Interview Gold:** In a modern JS project, **TypeScript compiler (tsc) or Babel transpiles**, and **Vite or Webpack bundles**. Vite is fast because it uses native ES modules in development and only bundles for production.

---

## **1️⃣8️⃣ JWKS (JSON Web Key Set)**

A JSON file containing public keys used to verify JWT tokens in authentication systems.

**Simple Explanation:** When an auth provider (like Auth0 or Google) issues a JWT, they sign it with their **private key**. To verify the token is genuine, your server needs the matching **public key**. JWKS is the publicly-accessible URL where those public keys live.

**Why not just use a shared secret?**
With a shared secret (`HS256`), both the issuer and verifier know the same key — if it leaks, anyone can forge tokens. With JWKS (`RS256`), the private key never leaves the auth server. Your app only has the public key, which can only *verify*, never *create* tokens.

**How it works (4 steps):**
```
1. User logs in → Auth server signs JWT with private key
2. Auth server publishes public keys at: https://auth.example.com/.well-known/jwks.json
3. Your API receives a JWT in the Authorization header
4. Your API fetches the public key from the JWKS URL and verifies the signature
```

```javascript
// Using jwks-rsa + jsonwebtoken in Node.js
const jwksClient = require("jwks-rsa");
const jwt = require("jsonwebtoken");

const client = jwksClient({ jwksUri: "https://YOUR_DOMAIN/.well-known/jwks.json" });

function getKey(header, callback) {
  client.getSigningKey(header.kid, (err, key) => {
    callback(null, key.getPublicKey());
  });
}

jwt.verify(token, getKey, { algorithms: ["RS256"] }, (err, decoded) => {
  console.log(decoded); // { userId: "123", role: "admin" }
});
```

> [!TIP]
> **Interview Gold:** JWKS enables **zero-secret-sharing** verification. Your microservices can verify tokens from the auth server without ever needing access to the signing secret — they just call the JWKS endpoint.

---

## **1️⃣9️⃣ Event Delegation**

Adding one event listener to a parent instead of many to children, utilizing **event bubbling**.

**Simple Explanation:** When you click a button, the click event doesn't just fire on the button — it "bubbles up" through every parent element. Event delegation exploits this: instead of adding a listener to every button, you add one listener to their parent and check which child was clicked.

**Without delegation** — 1 listener per button (bad for large/dynamic lists):
```javascript
// ❌ If you have 1000 items, this creates 1000 listeners
document.querySelectorAll(".todo-item").forEach(item => {
  item.addEventListener("click", handleClick);
});
// Also breaks for dynamically added items!
```

**With delegation** — 1 listener on the parent (fast, handles dynamic items):
```javascript
// ✅ One listener handles all current AND future items
document.getElementById("todo-list").addEventListener("click", (event) => {
  if (event.target.matches(".todo-item")) {
    console.log("Clicked:", event.target.textContent);
  }
});
```

> [!TIP]
> **Interview Gold:** Event delegation is the correct pattern for any list where items are added/removed dynamically (like a todo app or chat feed). It's also more memory-efficient — one listener uses much less memory than thousands.

---

## **2️⃣0️⃣ null vs undefined**

* **`undefined`:** Declared but not assigned.
* **`null`:** Intentional assignment of "no value".

**Simple Explanation:** `undefined` means "JavaScript hasn't given this a value yet." `null` means "a developer intentionally set this to nothing."

```javascript
let x;           // undefined — declared, no value given
let y = null;    // null — intentionally empty

// When JS assigns undefined automatically:
function greet(name) {
  console.log(name); // undefined if called as greet()
}

const obj = { a: 1 };
console.log(obj.b);   // undefined — property doesn't exist

// typeof comparison
typeof undefined  // "undefined"
typeof null       // "object"  ← famous JS quirk / bug

// Equality trap
null == undefined   // true  (loose equality)
null === undefined  // false (strict equality)
```

> [!TIP]
> **Interview Gold:** `typeof null === "object"` is a historic JavaScript bug that can never be fixed for backwards compatibility. Always use `=== null` to check for null, never `typeof x === "object"`.

---

---

## **2️⃣1️⃣ Optional Chaining (`?.`) & Nullish Coalescing (`??`)**

Two modern operators that make working with possibly-missing data much cleaner.

### **Optional Chaining `?.`**
Safely access deeply nested properties without crashing if something in the chain is `null` or `undefined`.
```javascript
const user = { profile: { name: "Alice" } };

console.log(user.profile?.name);    // "Alice"
console.log(user.address?.city);    // undefined  (no crash)
```

### **Nullish Coalescing `??`**
Returns the right-hand value only if the left side is `null` or `undefined` (not `0` or `""`).
```javascript
const name = null ?? "Guest";    // "Guest"
const count = 0 ?? 10;          // 0  (because 0 is NOT null/undefined)
const count2 = 0 || 10;         // 10 (|| treats 0 as falsy — different!)
```

> [!TIP]
> **Interview Gold:** Use `??` instead of `||` when `0` or `""` are valid values you don't want to override. Use `?.` to safely drill into API responses without a chain of `if` checks.

---

## **2️⃣2️⃣ Generators & Iterators**

### **Iterator**
An object that knows how to produce values one at a time. It has a `next()` method that returns `{ value, done }`.

### **Generator**
A function that can **pause** and **resume** execution. Written with `function*` and uses `yield` to pause.

```javascript
function* counter() {
  yield 1;
  yield 2;
  yield 3;
}

const gen = counter();
console.log(gen.next()); // { value: 1, done: false }
console.log(gen.next()); // { value: 2, done: false }
console.log(gen.next()); // { value: 3, done: false }
console.log(gen.next()); // { value: undefined, done: true }
```

* **Lazy evaluation:** Values are only computed when asked for.
* **Use cases:** Infinite sequences, custom data streams, paginated API fetching.

> [!TIP]
> **Interview Gold:** Generators are the foundation of `async/await` under the hood. They allow JavaScript to "pause" a function without blocking the thread.

---

## **2️⃣3️⃣ WeakMap & WeakSet vs Map & Set**

### **Map & Set**
* `Map`: Key-value store where keys can be any type (not just strings).
* `Set`: Collection of unique values.
* Both hold **strong references** — stored objects won't be garbage collected.

### **WeakMap & WeakSet**
* Only accept **objects as keys/values**.
* Hold **weak references** — if the object has no other references, it gets garbage collected automatically.
* Not iterable (no `.forEach`, no `.size`).

```javascript
let obj = { name: "Alice" };
const weakMap = new WeakMap();
weakMap.set(obj, "some data");

obj = null; // obj is garbage collected, weakMap entry disappears automatically
```

> [!TIP]
> **Interview Gold:** Use `WeakMap` for **private data attached to DOM nodes** or class instances. When the node is removed from the DOM, the WeakMap data is automatically cleaned up — no memory leaks.

---

## **2️⃣4️⃣ Async/Await Error Handling**

Always wrap `await` calls in `try/catch` to handle errors cleanly.

### **Basic pattern**
```javascript
async function fetchUser(id) {
  try {
    const res = await fetch(`/api/users/${id}`);
    if (!res.ok) throw new Error(`HTTP error: ${res.status}`);
    return await res.json();
  } catch (error) {
    console.error("Failed to fetch user:", error.message);
    return null;
  }
}
```

### **Global unhandled rejections (Node.js)**
```javascript
process.on("unhandledRejection", (reason, promise) => {
  console.error("Unhandled rejection:", reason);
  process.exit(1);
});
```

### **Promise.allSettled vs Promise.all**
* `Promise.all` — fails fast if **any** promise rejects.
* `Promise.allSettled` — waits for all, returns each result with `{ status: 'fulfilled' | 'rejected' }`.

> [!TIP]
> **Interview Gold:** Never use `async/await` without `try/catch`. A forgotten `await` in an `async` function returns a pending Promise — the function won't crash but will silently return the wrong value.

---

## **2️⃣5️⃣ Proxy & Reflect**

### **Proxy**
Wraps an object and lets you intercept and customize operations on it (get, set, delete, etc.).

```javascript
const user = { name: "Alice", age: 25 };

const safeUser = new Proxy(user, {
  get(target, key) {
    return key in target ? target[key] : `Property "${key}" not found`;
  },
  set(target, key, value) {
    if (key === "age" && typeof value !== "number") {
      throw new TypeError("Age must be a number");
    }
    target[key] = value;
    return true;
  },
});

console.log(safeUser.name);   // "Alice"
console.log(safeUser.email);  // 'Property "email" not found'
safeUser.age = "old";         // TypeError: Age must be a number
```

### **Reflect**
A built-in object with methods that mirror Proxy traps. Used inside Proxy handlers to forward the default behaviour.
```javascript
set(target, key, value) {
  console.log(`Setting ${key}`);
  return Reflect.set(target, key, value); // do the default set
}
```

> [!TIP]
> **Interview Gold:** Proxies power Vue 3's reactivity system and libraries like `immer`. They let you observe changes to objects without modifying the original object at all.
