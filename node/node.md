---
layout: default
title: Node.js
nav_order: 3
---

# 🟢 Node.js Interview Questions

A deep dive into Node.js architecture, the Event Loop, performance optimization, and backend best practices.

---

## 📑 Table of Contents
- [1️⃣ Single-Threaded Nature](#1️⃣-single-threaded-nature)
- [2️⃣ The Event Loop](#2️⃣-the-event-loop)
- [3️⃣ Core Architecture (libuv)](#3️⃣-core-architecture-libuv)
- [4️⃣ process.nextTick() vs setImmediate()](#4️⃣-processnexttick-vs-setimmediate)
- [5️⃣ Streams & Backpressure](#5️⃣-streams--backpressure)
- [6️⃣ Middleware in Express](#6️⃣-middleware-in-express)
- [7️⃣ Memory Management & Leaks](#7️⃣-memory-management--leaks)
- [8️⃣ Scaling Node.js](#8️⃣-scaling-nodejs)
- [9️⃣ Callbacks vs Promises vs Async/Await](#9️⃣-callbacks-vs-promises-vs-asyncawait)
- [🔟 JWT & Authentication](#🔟-jwt--authentication)
- [1️⃣1️⃣ Buffers](#1️⃣1️⃣-buffers)
- [1️⃣2️⃣ Multi-Threading: Worker Threads, Cluster, & Child Processes](#1️⃣2️⃣-multi-threading-worker-threads-cluster--child-processes)
- [1️⃣3️⃣ Stateless vs Stateful Systems](#1️⃣3️⃣-stateless-vs-stateful-systems)
- [1️⃣4️⃣ OAuth & Authorization](#1️⃣4️⃣-oauth--authorization)
- [1️⃣5️⃣ Real-World Scenarios & System Design](#1️⃣5️⃣-real-world-scenarios--system-design)
- [1️⃣6️⃣ module.exports vs exports](#1️⃣6️⃣-moduleexports-vs-exports)
- [1️⃣7️⃣ Handling Uncaught Exceptions](#1️⃣7️⃣-handling-uncaught-exceptions)

---

## **1️⃣ Single-Threaded Nature**

* **Only one JavaScript thread:** Only one JS function runs at a time. Node cannot run two JS calculations simultaneously on the main thread.
* **Concurrency vs Execution:** Node handles many operations at once (concurrency) but executes JS one at a time.

> [!TIP]
> **Interview Gold:** Node.js runs JavaScript on a single thread but handles many operations concurrently by using non-blocking I/O and an event-driven event loop. Node doesn’t do the waiting — it delegates and comes back later.

---

## **2️⃣ The Event Loop**

The event loop is a mechanism that lets JavaScript handle async tasks without blocking the main thread.

### **Main Parts:**
1. **Call Stack:** Where functions are executed. Only one runs at a time.
2. **Web APIs / Node APIs:** Handles async work like `setTimeout`, HTTP requests, file I/O. Runs outside the main thread.
3. **Task Queues:**
    * **Microtask Queue:** Promises (`.then`, `await`), `process.nextTick`.
    * **Callback / Macrotask Queue:** `setTimeout`, `setInterval`, I/O.
4. **Event Loop:** Constantly checks if the call stack is empty. If yes, it moves tasks from queues to the stack.

> [!IMPORTANT]
> **Priority:** Microtasks run before Macrotasks.

---

## **3️⃣ Core Architecture (libuv)**

* **Why is Node.js fast?** It uses non-blocking I/O and an event-driven architecture to handle many requests with a single thread.
* **What is libuv?** A C library that Node.js uses to handle async operations like file system, networking, and the event loop.
* **How does it handle async?** By offloading work to the OS or libuv thread pool and using callbacks/promises to handle results later.

---

## **4️⃣ process.nextTick() vs setImmediate()**

* **`process.nextTick()`:** Runs **immediately** after the current operation completes, before the event loop continues to the next phase. It has the highest priority.
* **`setImmediate()`:** Runs in the **check phase** of the event loop (after I/O events).

> [!NOTE]
> **Why use `setImmediate`?** To defer execution until after the current loop phase, ensuring the function runs asynchronously without blocking.
> **Why use `process.nextTick`?** Ideal for handling errors or cleanup tasks that must happen *before* any other async tasks.

---

## **5️⃣ Streams & Backpressure**

Streams handle data piece by piece instead of loading everything into memory.

### **Types of Streams:**
1. **Readable:** Reading data (e.g., `fs.createReadStream`, HTTP request).
2. **Writable:** Writing data (e.g., `fs.createWriteStream`, HTTP response).
3. **Duplex:** Both read and write (e.g., TCP sockets).
4. **Transform:** Modifies data while passing through (e.g., zlib, crypto).

> [!IMPORTANT]
> **Backpressure:** Happens when a writable stream cannot process incoming data fast enough. Node handles this by pausing the readable stream.

---

## **6️⃣ Middleware in Express**

Middleware is a function that runs between the request and response. it can:
* Execute any code.
* Make changes to the request and response objects.
* End the request-response cycle.
* Call the next middleware in the stack.

---

## **7️⃣ Memory Management & Leaks**

* **V8 Garbage Collector:** Automatically manages memory.
* **Causes of Leaks:**
    * Global variables.
    * Unremoved event listeners.
    * Large in-memory caches.
    * Closures holding references.

---

## **8️⃣ Scaling Node.js**

Scaling can be achieved through:
* **Cluster Module:** Multiple processes on the same machine.
* **Load Balancers:** Nginx, AWS ALB.
* **Microservices:** Decoupling functionality.
* **Container Orchestration:** Kubernetes, Docker Swarm.

---

## **9️⃣ Callbacks vs Promises vs Async/Await**

* **Callback:** Function passed as an argument, called after an async task finishes.
* **Callback Hell:** Deeply nested callbacks.
* **Avoidance:** Use Promises and Async/Await for cleaner, linear code.

---

## **10️⃣ JWT & Authentication**

**JWT (JSON Web Token)** is used for secure, stateless identification.
* **Header:** Algorithm & Token type.
* **Payload:** User data (claims like `userId`, `role`).
* **Signature:** Verification part created with a secret key.

---

## **1️⃣1️⃣ Buffers**

A **Buffer** is a way to handle raw binary data (bytes) directly in Node.js. It's essential for working with files, images, and network data.

---

## **1️⃣2️⃣ Multi-Threading: Worker Threads, Cluster, & Child Processes**

| Feature | Worker Threads | Cluster | Child Process |
| :--- | :--- | :--- | :--- |
| **Model** | Parallel threads in 1 process | Multiple processes | Separate process |
| **Memory** | Shared (SharedArrayBuffer) | Separate | Separate |
| **Best For** | CPU-intensive JS tasks | Scaling across CPU cores | Running non-JS/External scripts |

---

## **1️⃣3️⃣ Stateless vs Stateful Systems**

* **Stateful:** Remembers past interactions (e.g., server-side sessions).
* **Stateless:** Each request is independent (e.g., JWT). Preferred for horizontal scaling.

---

## **1️⃣4️⃣ OAuth & Authorization**

OAuth is an **authorization** framework (not authentication). It lets users grant limited access to their data on one app to another without sharing passwords (e.g., "Login with Google").

---

## **1️⃣5️⃣ Real-World Scenarios & System Design**

### **Scenario: Slow API under high traffic**
1. Identify bottleneck (CPU/DB/IO).
2. Add caching (Redis).
3. Optimize DB queries/indexes.
4. Horizontal scaling.

### **Scenario: Frequent Crashes**
* Use PM2 for auto-restarts.
* Implement graceful shutdown.
* Use centralized logging (Winston/Morgan).

### **Scenario: Invalidating JWT**
Since JWT is stateless, invalidation requires:
* Short expiry + Refresh tokens.
* Blacklisting tokens in Redis.

---

## **1️⃣6️⃣ module.exports vs exports**

* **`module.exports`:** The actual object returned by `require()`.
* **`exports`:** A shorthand reference to `module.exports`. Reassigning `exports` breaks the link.

---

## **1️⃣7️⃣ Handling Uncaught Exceptions**

Always use `process.on('uncaughtException')` for cleanup, but follow it with `process.exit(1)` and let a process manager like PM2 restart the instance to ensure a clean state.
