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
- [1️⃣8️⃣ WebSocket vs HTTPS](#1️⃣8️⃣-websocket-vs-https)
- [1️⃣9️⃣ How Node.js Handles High Concurrency (The Waiter Analogy)](#1️⃣9️⃣-how-nodejs-handles-high-concurrency-the-waiter-analogy)

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

**Simple Explanation:** Think of a garden hose. If you open the tap fully but cover the nozzle with your thumb (slow output), water backs up — that's backpressure. Node's `pipe()` handles it automatically by slowing the source when the destination is full.

**Reading a large file without loading it all into memory:**
```javascript
const fs = require("fs");

// ❌ Bad — loads entire 1GB file into RAM
const data = fs.readFileSync("huge-file.csv");

// ✅ Good — reads chunk by chunk, memory stays low
const readable = fs.createReadStream("huge-file.csv", { encoding: "utf8" });
readable.on("data", (chunk) => {
  console.log("Got chunk:", chunk.length, "bytes");
});
readable.on("end", () => console.log("Done"));
```

**`pipe()` — the simplest way to connect streams (handles backpressure automatically):**
```javascript
// Copy a file efficiently, no matter how big it is
fs.createReadStream("input.txt")
  .pipe(fs.createWriteStream("output.txt"));
```

> [!TIP]
> **Interview Gold:** Use streams when working with large files, video uploads, or real-time data. Without streams, a 1GB file upload would load 1GB into RAM — your server runs out of memory. With streams, memory usage stays constant regardless of file size.

---

## **6️⃣ Middleware in Express**

Middleware is a function that runs between the request and response. it can:
* Execute any code.
* Make changes to the request and response objects.
* End the request-response cycle.
* Call the next middleware in the stack.

**Simple Explanation:** Think of middleware as airport security checkpoints. Every passenger (request) must pass through each checkpoint in order before reaching the gate (route handler). Each checkpoint can stop the passenger, modify their luggage, or wave them through.

**Middleware signature:** `(req, res, next) => {}` — always call `next()` to pass control forward, or `res.send()` to end the cycle.

```javascript
const express = require("express");
const app = express();

// 1. Logger middleware — runs for every request
app.use((req, res, next) => {
  console.log(`${req.method} ${req.url} — ${new Date().toISOString()}`);
  next(); // pass to next middleware
});

// 2. Auth middleware — blocks unauthorized requests
function requireAuth(req, res, next) {
  const token = req.headers.authorization;
  if (!token) return res.status(401).json({ error: "Unauthorized" });
  next(); // token exists, continue
}

// 3. Route handler — only reached if auth passes
app.get("/dashboard", requireAuth, (req, res) => {
  res.json({ message: "Welcome to dashboard" });
});
```

**Order matters** — middleware runs top to bottom. If you put auth after the route, it never runs for that route.

> [!TIP]
> **Interview Gold:** Middleware is the Express answer to "how do you avoid repeating auth/logging/validation code in every route?" Apply it globally with `app.use()` or per-route as the second argument to `app.get()`.

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

**Simple Explanation:** Strings in JavaScript are text. But files, images, and network packets are raw bytes — not text. A Buffer is like a fixed-size box of bytes that lets you work with that binary data directly, before it's converted to a string.

```javascript
// Create a Buffer from a string
const buf1 = Buffer.from("Hello, World!", "utf8");
console.log(buf1);           // <Buffer 48 65 6c 6c 6f ...>
console.log(buf1.toString()); // "Hello, World!"
console.log(buf1.length);    // 13 (bytes)

// Allocate an empty Buffer of 10 bytes
const buf2 = Buffer.alloc(10);   // <Buffer 00 00 00 00 ...> — zeroed out
const buf3 = Buffer.allocUnsafe(10); // faster but contains old memory data

// Convert between encodings
const encoded = Buffer.from("Hello").toString("base64"); // "SGVsbG8="
const decoded = Buffer.from("SGVsbG8=", "base64").toString("utf8"); // "Hello"
```

**When you'll use Buffers:**
* Reading binary files (`fs.readFile` returns a Buffer by default)
* Uploading/downloading images over HTTP
* Cryptography (`crypto.createHash` works with Buffers)
* WebSockets sending binary frames

> [!TIP]
> **Interview Gold:** When `fs.readFile` gives you back data without specifying an encoding, it's a Buffer — raw bytes. Pass `{ encoding: 'utf8' }` to get a string instead. Always know which one you're working with or you'll end up logging `<Buffer 68 65 ...>` and wondering why.

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

**Simple Explanation:** A **stateful** server is like a waiter who remembers your order. If you get a different waiter mid-meal, they have no idea what you ordered. A **stateless** server is like an order ticket — the ticket (JWT) travels with you and any waiter can read it.

```javascript
// ❌ Stateful — session stored on THIS server's memory
// If user goes to Server B, Server B has no session
app.post("/login", (req, res) => {
  req.session.userId = user.id; // stored in server memory
  res.send("Logged in");
});

// ✅ Stateless — JWT travels with the client
// Any server can verify it without shared memory
app.post("/login", (req, res) => {
  const token = jwt.sign({ userId: user.id }, process.env.JWT_SECRET);
  res.json({ token }); // client stores and sends token with every request
});

// Any server can verify
app.get("/dashboard", (req, res) => {
  const decoded = jwt.verify(req.headers.authorization, process.env.JWT_SECRET);
  res.json({ user: decoded.userId });
});
```

> [!TIP]
> **Interview Gold:** Stateless systems scale horizontally with ease — add 10 servers behind a load balancer and any server can handle any request. Stateful systems require "sticky sessions" (always routing the same user to the same server), which is fragile and limits scaling.

---

## **1️⃣4️⃣ OAuth & Authorization**

OAuth is an **authorization** framework (not authentication). It lets users grant limited access to their data on one app to another without sharing passwords (e.g., "Login with Google").

**Simple Explanation:** Think of OAuth like a hotel key card. The hotel (Google/GitHub) issues a key card (access token) to your app. Your app uses that key card to open specific doors (read your email, see your profile) — without ever knowing your room's master password.

**OAuth 2.0 Authorization Code Flow (the most common, most secure):**
```
1. User clicks "Login with Google" on your app
   → Your app redirects to: https://accounts.google.com/o/oauth2/auth?client_id=...&scope=email

2. User sees Google's consent screen: "App wants to read your email — Allow?"
   → User clicks Allow
   → Google redirects back to your app with a short-lived CODE

3. Your server exchanges the CODE for tokens (done server-side, never in browser):
   POST https://oauth2.googleapis.com/token
   { code, client_id, client_secret, redirect_uri }
   → Google returns: { access_token, refresh_token, expires_in }

4. Your app uses the access_token to call Google APIs:
   GET https://www.googleapis.com/oauth2/v1/userinfo
   Authorization: Bearer <access_token>
   → Returns: { email: "user@gmail.com", name: "Alice" }
```

**Key terms:**
* **`access_token`** — short-lived (1 hour). Used to call APIs.
* **`refresh_token`** — long-lived. Used to get a new access_token when it expires.
* **`scope`** — what permissions you're requesting (`email`, `profile`, `read:repos`).

> [!TIP]
> **Interview Gold:** OAuth = "What are you allowed to do?" (Authorization). OpenID Connect (OIDC) = "Who are you?" (Authentication). "Login with Google" uses **both** — OAuth for access + OIDC for the user's identity (the `id_token`).

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

**Simple Explanation:** At the start of every Node module, `exports` and `module.exports` point to the same object. But they're separate variables. If you reassign `exports = something`, you break that link — `module.exports` still points to the original empty object, and that's what `require()` returns.

```javascript
// ✅ WORKS — adding properties to the shared object
exports.greet = () => "Hello";
exports.bye = () => "Goodbye";
// require() gets: { greet: fn, bye: fn }

// ✅ WORKS — replacing module.exports entirely
module.exports = function() { return "I am the export"; };
// require() gets: the function itself

// ❌ BROKEN — reassigning exports cuts the link to module.exports
exports = { greet: () => "Hello" };
// require() gets: {} (empty! module.exports is unchanged)
```

**Rule of thumb:**
* Use `exports.name = value` to add multiple named exports.
* Use `module.exports = value` to export a single thing (a class, a function, an object).

> [!TIP]
> **Interview Gold:** When in doubt, always use `module.exports`. It's the source of truth. `exports` is just a convenience shortcut that breaks the moment you reassign it.

---

## **1️⃣7️⃣ Handling Uncaught Exceptions**

Always use `process.on('uncaughtException')` for cleanup, but follow it with `process.exit(1)` and let a process manager like PM2 restart the instance to ensure a clean state.

**Simple Explanation:** An uncaught exception is a crash that no `try/catch` caught. The process is now in an unknown state — memory might be corrupt, connections might be half-open. The only safe move is to log it, do any cleanup (close DB connections), and **restart cleanly**. Never try to keep running after an uncaught exception.

```javascript
// ❌ Bad — swallowing the error and continuing in a broken state
process.on("uncaughtException", (err) => {
  console.error("Error:", err.message);
  // no exit — process keeps running in unknown state
});

// ✅ Good — log, cleanup, exit
process.on("uncaughtException", (err) => {
  console.error("FATAL — uncaught exception:", err);
  // do cleanup: close DB connections, flush logs
  db.disconnect().finally(() => {
    process.exit(1); // exit with error code — PM2/Docker will restart
  });
});

// For unhandled Promise rejections (async equivalent)
process.on("unhandledRejection", (reason, promise) => {
  console.error("Unhandled promise rejection:", reason);
  process.exit(1);
});
```

**With PM2** — auto-restart on crash:
```bash
pm2 start app.js --name my-api   # starts the app
# if process.exit(1) is called, PM2 restarts it automatically
```

> [!TIP]
> **Interview Gold:** `uncaughtException` is your **last resort** safety net, not a try/catch replacement. In production, always use both: PM2 or Kubernetes for auto-restart, AND proper `try/catch` in your async code to handle expected errors gracefully.

---

## **1️⃣8️⃣ WebSocket vs HTTPS**

### **Technical Breakdown**

| Feature | HTTPS (Hypertext Transfer Protocol Secure) | WebSocket |
| :--- | :--- | :--- |
| **Communication** | Unidirectional (Client-to-Server request) | Bidirectional (Full-duplex) |
| **Connection** | Short-lived (Stateless) | Long-lived (Persistent) |
| **Header Overhead** | High (Headers sent with every request) | Low (Headers sent once during handshake) |
| **Real-time** | Low (Requires polling/long-polling) | High (Instant data push) |
| **Use Case** | REST APIs, fetching data, static assets | Chat apps, live scores, stock tickers |

### **Interview Scenario: "How would you explain the difference between WebSockets and HTTPS to a non-technical stakeholder?"**

**Answer:**
Think of **HTTPS** like a **Post Office**. If you want information, you have to send a letter (Request). The server reads it and sends a reply back (Response). Once the reply is delivered, the connection is closed. If you want more info, you have to send another letter.

**WebSockets** are like a **Phone Call**. Once the call is connected (Handshake), the line stays open. Both people can talk at the same time, and you don't have to dial the number again every time you want to say something. It’s perfect for situations where data needs to flow instantly and constantly, like a live chat.

### **How it works in Node.js:**
1. **Handshake:** The client sends a standard HTTP request with an `Upgrade: websocket` header.
2. **Upgrade:** If the server supports it, it switches the protocol from HTTP to WebSocket.
3. **Persistence:** The TCP connection remains open, allowing the server to push data to the client without a new request.

> [!TIP]
> **Interview Gold:** Use WebSockets when you need **low-latency, real-time updates**. Use HTTPS for standard **CRUD operations** where data doesn't change every second.

---

## **1️⃣9️⃣ How Node.js Handles High Concurrency (The Waiter Analogy)**

**The Question:** "If Node.js is single-threaded, how can it handle thousands of users at once without crashing or slowing down?"

**The Simple Explanation (The Restaurant Analogy):**

Imagine a restaurant with **one waiter** (The Single Thread) and a **kitchen** (The Operating System/Worker Pool).

1. **Standard Approach (Multi-threaded):** Every time a guest arrives, you hire a new waiter. This is expensive and uses a lot of space (Memory/CPU overhead).
2. **Node.js Approach (Single-threaded Event Loop):**
   - The waiter takes an order from Table A and sends it to the kitchen.
   - Instead of standing there waiting for the food, the waiter immediately goes to Table B to take their order.
   - When the food for Table A is ready, a bell rings (Event), and the waiter goes back to pick it up and deliver it.

**Why it works:** The waiter is never "idle" waiting for the kitchen. They are always moving, taking orders, or delivering food. The "waiting" happens in the kitchen, not by the waiter.

**Core Idea:** Node.js doesn't wait for I/O (Database, File System, Network). It delegates the task and moves to the next request, coming back only when the task is finished.

> [!TIP]
> **Interview Gold:** This is called **Non-blocking I/O**. It makes Node.js extremely efficient for I/O-heavy applications (like Chat or Streaming) but less ideal for heavy math calculations that would keep the "waiter" stuck at one table for too long.

---

## **2️⃣0️⃣ CORS (Cross-Origin Resource Sharing)**

**What it is:** A browser security rule. When your frontend (e.g., `localhost:3000`) calls a backend on a different origin (e.g., `localhost:5000` or `api.example.com`), the browser blocks the request unless the server explicitly allows it.

* **Origin** = protocol + domain + port. `http://localhost:3000` and `http://localhost:5000` are different origins.
* CORS only affects **browser** requests. Postman and server-to-server calls are never blocked.

### **How to fix it in Node/Express**
```javascript
const cors = require("cors");

app.use(cors({
  origin: "https://myfrontend.com", // allow only this origin
  methods: ["GET", "POST"],
}));
```

### **Preflight request**
For non-simple requests (PUT, DELETE, custom headers), the browser sends an `OPTIONS` request first to check if the server allows it. Your server must respond with the right headers.

> [!TIP]
> **Interview Gold:** CORS is enforced by the **browser**, not the server. The server just sends headers saying "I allow this." If you bypass CORS using a proxy, you're avoiding the browser check — the backend never sees a difference.

---

## **2️⃣1️⃣ Rate Limiting**

**What it is:** Restricting how many requests a client can make in a given time window to protect your API from abuse and overload.

### **Common algorithms**
* **Fixed Window:** Allow N requests per minute. Resets at the top of every minute. Simple but has edge-case spikes at window boundaries.
* **Sliding Window:** Tracks requests over a rolling time window. Smoother, no boundary spikes.
* **Token Bucket:** Each client has a bucket of tokens. Each request costs one token. Tokens refill at a constant rate. Allows short bursts.

### **In Express (using `express-rate-limit`)**
```javascript
const rateLimit = require("express-rate-limit");

const limiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100,                  // max 100 requests per window per IP
  message: "Too many requests, please try again later.",
});

app.use("/api/", limiter);
```

> [!TIP]
> **Interview Gold:** Always rate-limit **authentication endpoints** (login, forgot-password) to prevent brute-force attacks. For public APIs, rate limit by IP; for authenticated APIs, rate limit by user ID.

---

## **2️⃣2️⃣ REST API Design Principles**

A well-designed REST API is predictable and easy to use.

### **Key rules**
* **Use nouns, not verbs in URLs:** `/users` not `/getUsers`
* **Use HTTP methods correctly:**
  * `GET` — read, `POST` — create, `PUT/PATCH` — update, `DELETE` — delete
* **Use proper status codes:**
  * `200` OK, `201` Created, `400` Bad Request, `401` Unauthorized, `403` Forbidden, `404` Not Found, `500` Server Error
* **Version your API:** `/api/v1/users` so you can make breaking changes without breaking existing clients.
* **Use plural nouns:** `/users/123` not `/user/123`

### **Example**
```
GET    /api/v1/users          → list all users
POST   /api/v1/users          → create a user
GET    /api/v1/users/123      → get user 123
PATCH  /api/v1/users/123      → update user 123
DELETE /api/v1/users/123      → delete user 123
```

> [!TIP]
> **Interview Gold:** `PUT` replaces the entire resource; `PATCH` updates only the changed fields. In practice, most APIs use `PATCH` for updates because you rarely want to replace everything.

---

## **2️⃣3️⃣ Environment Variables & `.env` Management**

Environment variables store config that changes between environments (dev, staging, prod) — like API keys, DB URLs, secrets.

### **Why not hardcode?**
* Secrets in code end up in version control → security risk.
* Same code, different config per environment.

### **In Node.js**
```javascript
// .env file (never commit this!)
DATABASE_URL=postgres://localhost/mydb
JWT_SECRET=supersecretkey

// Load with dotenv
require("dotenv").config();
console.log(process.env.DATABASE_URL);
```

### **Rules**
* Add `.env` to `.gitignore`.
* Commit a `.env.example` with placeholder values so teammates know what's needed.
* In production, inject env vars through the platform (Vercel, Railway, AWS Secrets Manager) — never deploy a `.env` file.

> [!TIP]
> **Interview Gold:** Never store secrets in code or Docker images. Use a secrets manager (AWS Secrets Manager, HashiCorp Vault) in production. Rotate secrets regularly and audit who has access.
