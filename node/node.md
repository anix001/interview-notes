---
layout: default
title: Node.js
nav_order: 3
---

# 🟢 Node.js Interview Questions

Master the internals of Node.js, including the Event Loop, asynchronous I/O, and server-side performance.

---

## 📑 Table of Contents
- [Architecture: Single-Threaded Nature](#-architecture-single-threaded-nature)
- [The Event Loop](#-the-event-loop)
- [Node.js Core (libuv & Process)](#-nodejs-core-libuv--process)
- [Streams & Buffers](#-streams--buffers)
- [Concurrency: Worker Threads & Cluster](#-concurrency-worker-threads--cluster)
- [Security: JWT & OAuth](#-security-jwt--oauth)
- [Scaling & Performance](#-scaling--performance)

---

## 🏗️ Architecture: Single-Threaded Nature

Node.js is **single-threaded** for JavaScript execution but **concurrent** for I/O operations.
* **Execution:** Only one JS function runs at a time on the main thread.
* **Concurrency:** achieved via the **Event Loop** and **Non-blocking I/O**.

---

## 🔄 The Event Loop

The heart of Node.js that coordinates asynchronous tasks.

### Core Phases:
1. **Timers:** Executes `setTimeout` and `setInterval`.
2. **I/O Callbacks:** Executes almost all callbacks (except timers and close).
3. **Poll:** Retrieves new I/O events.
4. **Check:** Executes `setImmediate`.
5. **Close:** Executes `on('close')` events.

> [!IMPORTANT]
> **Priority Queues:** `process.nextTick()` and Promises (Microtasks) have higher priority than the main Event Loop phases and are executed immediately after the current operation finishes.

---

## 🛠️ Node.js Core (libuv & Process)

### libuv
A C-based library that provides the Event Loop and handles the **Thread Pool** for heavy tasks like file I/O, DNS lookups, and crypto.

### `process.nextTick()` vs `setImmediate()`
* **`process.nextTick()`:** Runs **before** the event loop continues to the next phase.
* **`setImmediate()`:** Runs in the **Check** phase of the event loop.

---

## 📦 Streams & Buffers

### Streams
Handle data piece-by-piece to save memory.
* **Readable:** `fs.createReadStream()`
* **Writable:** `fs.createWriteStream()`
* **Duplex:** Both read and write (e.g., Sockets).
* **Transform:** Modifies data while reading/writing (e.g., Gzip).

### Buffers
A raw memory allocation used to handle **binary data** directly (images, video, network packets).

---

## 🚀 Concurrency: Worker Threads & Cluster

| Feature | Worker Threads | Cluster Module |
| :--- | :--- | :--- |
| **Model** | Multiple threads in one process | Multiple processes |
| **Memory** | Shared memory | Separate memory |
| **Best For** | CPU-intensive tasks | Scaling web servers |
| **Communication** | MessagePort / SharedArrayBuffer | IPC (Inter-Process Communication) |

---

## 🔐 Security: JWT & OAuth

### JWT (JSON Web Token)
A stateless authentication method.
* **Header:** Algorithm & token type.
* **Payload:** User claims (ID, roles).
* **Signature:** Verification hash.

### OAuth
An **authorization** framework (e.g., "Login with Google") that allows third-party apps to access data without sharing passwords.

> [!TIP]
> **Interview Gold:** JWT is for **Authentication** (Who are you?), while OAuth is for **Authorization** (What can you access?).

---

## 📈 Scaling & Performance

### 1️⃣ Horizontal Scaling
Adding more machines or processes (Cluster/Kubernetes).

### 2️⃣ Vertical Scaling
Adding more CPU/RAM to a single machine.

### 3️⃣ Backpressure
Occurs when a **Readable** stream produces data faster than a **Writable** stream can consume it. Node.js handles this by pausing the source until the buffer clears.
