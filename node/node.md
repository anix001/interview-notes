**Node JS Interview Question:**

## **1️⃣ What “single-threaded” means in [Node.js](http://Node.js)**

* **Only one JavaScript thread**

* Only **one JS function runs at a time**

  So Node **cannot** run two JS calculations simultaneously on the main thread.

  **2️⃣ Then how does it handle many operations?**   
  **Event loop \+ non-blocking I/O**  
  Node **doesn’t wait** for slow things.

  ## **5️⃣ Concurrency vs execution (important)**

* Node **handles many operations at once** (concurrency)

* But **executes JS one at a time**


**Node.js runs JavaScript on a single thread but handles many operations concurrently by using non-blocking I/O and an event-driven event loop. Node doesn’t do the waiting — it delegates and comes back later.**

**What is the Event Loop?** 

**The event loop is a mechanism that lets JavaScript handle async tasks without blocking the main thread.**

**Main parts (interviewers love this)**

1. **Call Stack**

   * **Where functions are executed**

   * **Only one function runs at a time**

2. **Web APIs / Node APIs**

   * **Handles async work like `setTimeout`, HTTP requests, file I/O**

   * **Runs outside the main thread**

3. **Task Queues**

   * **Microtask Queue → promises (`.then`, `await`)**

   * **Callback / Macrotask Queue → `setTimeout`, `setInterval`, I/O**

4. **Event Loop**

   * **Constantly checks:**

     * **Is the call stack empty?**

     * **If yes → move tasks from queues to stack**  
       

**Microtasks run before Macrotasks.**

**1️⃣ Why is Node.js fast?**

**Node.js is fast because it uses non-blocking I/O and an event-driven architecture, so it can handle many requests with a single thread.**

**2️⃣ What is libuv?**

**libuv is a C library that Node.js uses to handle async operations like file system, networking, and the event loop.**

**3️⃣ How does Node.js handle async operations?**

**By offloading work to the OS or libuv thread pool and using callbacks, promises, or async/await to handle results later.**

## **4️⃣ Difference between `process.nextTick()` and `setImmediate()`?**

* **`process.nextTick()` runs before the event loop continues**

* **`setImmediate()` runs in the check phase**

**nextTick has higher priority and can block the loop if misused**

**why to use setimmediate if we can just call the function?**

**setImmediate() is used to defer function execution until after the current event loop phase completes, ensuring the function runs asynchronously without blocking the main thread.**

**Calling a function directly (synchronously) can lead to issues:**

**Blocking the event loop, especially during long-running or CPU-intensive operations.**

**Inconsistent callback behavior, where some callbacks run synchronously and others asynchronously, breaking expected flow.** 

**What does process.nextTick() do?**

**process.nextTick() schedules a callback function to run immediately after the current operation completes, but before the event loop continues to the next phase.**

**It ensures the callback executes as soon as possible, after the current call stack unwinds, making it ideal for:**

**Handling errors or cleanup tasks before the event loop proceeds.**

**Ensuring event listeners are set up before emitting events (e.g., in constructors).**

**Preventing I/O starvation by allowing critical operations to run before other async tasks.**

**Unlike setTimeout(() \=\> {}, 0), which waits for the next tick, process.nextTick() runs before any other asynchronous callbacks, including setImmediate() and timers, giving it the highest priority in the event loop.** 

## **5️⃣ What are streams in Node.js?**

**Answer:**

**Streams handle data piece by piece instead of loading everything into memory, making Node.js memory-efficient.**

**Example:**

* **Video streaming**

* **File upload/download**

### **1️⃣ Readable Stream**

**👉 Used to read data piece by piece.**

**Examples:**

* **Reading a file**

* **Incoming HTTP request**

### **2️⃣ Writable Stream**

**👉 Used to write data chunk by chunk.**

**Examples:**

* **Writing to a file**

* **HTTP response**

### **3️⃣ Duplex Stream**

**👉 Can read and write data.**

**Examples:**

* **TCP sockets**

* **WebSocket**

### **4️⃣ Transform Stream**

**👉 A special Duplex stream that modifies data while passing through.**

**Examples:**

* **gzip compression**

* **encryption**

* **changing data format**

## **6️⃣ What is backpressure?** 

**Answer:**

**Backpressure happens when a writable stream cannot process incoming data fast enough.**

## **8️⃣ What is middleware in Express?**

**Answer:**

**Middleware is a function that runs between request and response and can modify them or end the request.**

## **9️⃣ How does Node.js handle memory management?**

**Answer:**

**Node.js uses the V8 garbage collector to automatically manage memory.**

**.**

---

## **🔟 What causes memory leaks in Node.js?**

**Answer:**

* **Global variables**

* **Unremoved event listeners**

* **Large in-memory caches**

* **Closures holding references**

## **1️⃣1️⃣ What is the difference between `require` and `import`?**

**Answer:**

* **`require` → CommonJS**

* **`import` → ES Modules**

## **1️⃣2️⃣ How do you scale a Node.js application?**

**Answer:**

**Using cluster, load balancers, microservices, or container orchestration.**

## **1️⃣3️⃣ How does Node.js handle multiple CPU cores?**

**Answer:**

**Node.js uses clustering or worker threads to utilize multiple cores.**

## **1️⃣4️⃣ What is a callback hell and how do you avoid it?**

**A callback is a function passed as an argument to another function, which is called later, usually after an async task finishes.**

**👉 Node.js uses callbacks a lot because many operations are non-blocking (file read, DB call, API call).**

**Callback hell is deeply nested callbacks; avoid it using promises and async/await.**

**1️⃣5️⃣ What is JWT and where do you use it?**

**Answer:**

**JWT is a token-based authentication method used to securely identify users in stateless APIs.**

**Parts:**

**1️⃣ Header**

**👉 Tells how the token is created**

**Contains:**

* **Token type → `JWT`**

* **Algorithm → `HS256`, `RS256`**  
  **`2️⃣ Payload`**  
  **`👉 Contains user data (claims)`**  
  **`Examples:`**  
* **`userId`**

* **`role`**

* **`exp (expiry)`**

  ### **`3️⃣ Signature`**

  **`👉 Used to verify token authenticity`**  
  **`Created using:`**  
* **`Encoded Header + Payload`**

* **`Secret key / private key`**

## **`1️⃣6️⃣ What happens when a promise is rejected and not handled?`**

**`Answer:`**

**`It causes an unhandled rejection which may crash the Node process in newer versions.`**

## **`1️⃣7️⃣ How do you secure a Node.js app?`**

**`Answer:`**

* **`Use HTTPS`**

* **`Validate input`**

* **`Use helmet`**

* **`Avoid exposing secrets`**

* **`Use rate limiting`**

## **`1️⃣8️⃣ What is environment variable usage in Node.js?`**

**`Answer:`**

**`Environment variables store configuration values outside the code, like API keys.`**

**`NOTE:`**

**`Node.js is best for I/O-heavy applications but needs worker threads or multiple processes for CPU-heavy work.”`**

**`What is a Buffer? (simple)`**

**`A Buffer is a way to handle binary data directly in Node.js.`**

**`JavaScript normally works with strings and objects, but things like:`**

* **`files`**

* **`images`**

* **`videos`**

* **`network data`**

**`are binary, and Buffer is how Node handles them.`**

---

## **`📦 Why do we need Buffer?`**

**`Because:`**

* **`Data from files or network arrives as raw bytes`**

* **`JS alone can’t handle raw binary well`**

* **`Buffer lets Node read, write, and manipulate bytes`**

##  **`Worker Threads (for CPU-heavy tasks)`**

### **`What it is`**

**`Worker Threads run JavaScript in parallel threads inside the same process.`**

### **`Key points`**

* **`Multiple threads`**

* **`Share memory (via SharedArrayBuffer)`**

* **`Best for CPU-intensive tasks`**

### **`Example use cases`**

* **`Image processing`**

* **`Data encryption`**

* **`Heavy calculations`**

### **`Interview one-liner`**

**`“Worker threads allow true parallel execution of JavaScript for CPU-bound tasks.”`**

## **`2️⃣ Cluster (for scaling requests)`**

### **`What it is`**

**`Cluster creates multiple Node processes to handle more traffic.`**

### **`Key points`**

* **`Multiple processes`**

* **`Separate memory`**

* **`Uses round-robin load balancing`**

* **`One process per CPU core`**

### **`Example use cases`**

* **`API servers`**

* **`Web apps with many users`**

### **`Interview one-liner`**

**`“Cluster is used to scale Node.js apps across CPU cores by running multiple processes.”`**

**`“Node.js uses worker threads for CPU-intensive work and cluster for scaling across CPU cores. Both help overcome the single-threaded nature of JavaScript.”`**

## **`3️⃣ Child Processes (general purpose)`**

### **`What it is`**

**`Used to spawn external processes or scripts.`**

### **`Key points`**

* **`Separate process`**

* **`Can run shell commands`**

* **`IPC available`**

### **`Example`**

* **`Running Python scripts`**

* **`Video processing tools`**

## **`3️⃣ Child Processes (general purpose)`** 

**Child processes let Node.js run other programs or scripts outside the main process.**

         Node provides this via the **`child_process` module**.

          Why?

* Run CPU-heavy work

* Run other languages (Python, Bash)

* Prevent blocking main Node process

## **2️⃣ `spawn()` (most commonly used)**

### **What it does**

Starts a new process and **streams output**.

### **Key points**

* Non-blocking

* Good for **large output**

* Uses streams (`stdout`, `stderr`)

### **Example use cases**

* Video processing

* Running shell commands

* Continuous data output

### **Interview one-liner**

“spawn is used when you need streaming data from a child process.”

## **3️⃣ `exec()` (quick mention – interviewer may ask)**

Runs a command and buffers the output.

⚠️ Not asked by you, but useful:

* Bad for large output

* Risk of memory issues

## **4️⃣ `fork()` (Node-specific)**

### **What it does**

Creates a **new Node.js process** with built-in communication.

### **Key points**

* Special case of `spawn`

* Has **IPC channel**

* Best for running **another JS file**

### **Example use cases**

* Background jobs

* Task queues

* CPU-heavy JS work

### **Interview one-liner**

“fork is a special spawn for Node scripts with built-in IPC.”

### **IPC \= Inter-Process Communication**

IPC is a way for the parent and child processes to **send messages to each other**.

With `fork()`, IPC is **built-in automatically**.

##  **Interview-ready answer (short)**

“Yes, fork also supports streaming output. In addition, fork provides built-in IPC, which allows parent and child Node processes to exchange messages.”

## **🧠 Memory trick**

```bash
fork = spawn + IPC
```

**✅ Stateless vs Stateful (Simple Explanation)**

### **🧠 Stateful**

A **stateful** system **remembers past interactions**.

That means:

👉 The server keeps client data (session, login, cart, etc.) in memory or storage.

Example:

* You log in

* Server stores your session

* Every next request depends on that stored session

If the server restarts → state is lost.

### **⚡ Stateless**

A **stateless** system **does NOT remember anything** between requests.

Each request is **fully independent** and contains everything needed.

👉 No session stored on server.

Example:

* You send JWT token with every request

* Server validates token

* No memory of previous calls

If server restarts → nothing breaks.

### 

### **✅ What is OAuth?**

**OAuth is an authorization framework that lets users grant limited access to their data on one app to another app — without sharing their password.**

Example:

You click:

👉 **“Login with Google”**

You never give your Google password to that website.  
 Instead, Google gives the app a **token**.

That token \= permission.

## **✅ Very Simple Flow (Login with Google)**

1. User clicks **Login with Google**

2. App redirects user to Google

3. User approves permission

4. Google sends back an **authorization code**

5. Backend exchanges code for **access token**

6. Backend uses token to get user info

7. User is logged in

---

## **✅ Key Point (Interview Gold)**

OAuth is about **authorization**, not authentication.

Authentication \= who you are  
 Authorization \= what you can access

OAuth mainly handles **authorization**.

(Login is built on top of it.)

# **`✅ Scenario 1`**

### **`👉 You’re building Login with Google in a Node.js app. Explain the flow.`**

### **`🎯 Answer:`**

1. **`User clicks Login with Google`**

2. **`App redirects to Google OAuth`**

3. **`User consents`**

4. **`Google returns authorization code`**

5. **`Backend exchanges code for access token`**

6. **`Backend fetches user info`**

7. **`Creates local user if not exists`**

8. **`Issues JWT/session`**

**`This keeps passwords secure and uses token-based access.`**

### **`Interviewer testing:`**

**`OAuth understanding + backend flow.`**

**`✅ Scenario 2`**

### **`👉 Your API must scale to millions of users. Sessions or JWT?`**

### **`🎯 Answer:`**

**`JWT (stateless).`**

**`Reason:`**

* **`No server memory`**

* **`Easy load balancing`**

* **`No sticky sessions`**

* **`Horizontal scaling`**

**`Sessions require Redis and coordination.`**

# **`✅ Scenario 3`**

### **`👉 User logs out. How do you invalidate JWT?`**

### **`🎯 Answer:`**

**`Since JWT is stateless:`**

**`Options:`**

* **`Maintain blacklist in Redis`**

* **`Short token expiry`**

* **`Rotate refresh tokens`**

* **`Delete refresh token from DB`**

**`Best practice:`**  
 **`Short-lived access token + refresh token.`**

# **`✅ Scenario 4`**

### **`👉 OAuth token is stolen. What do you do?`**

### **`🎯 Answer:`**

* **`Revoke token`**

* **`Rotate secrets`**

* **`Force re-login`**

* **`Use HTTPS`**

* **`Short token expiry`**

* **`Use refresh token rotation`**

**`Defense in depth.`**

# **✅ Scenario 5**

### **👉 How do you protect Node APIs?**

### **🎯 Answer:**

* **JWT verification middleware**

* **Role based access (RBAC)**

* **Rate limiting**

* **Helmet**

* **Input validation**

* **CORS**

* **HTTPS**

# **✅ Scenario 6**

### **👉 Multiple Node servers — how to handle login?**

### **🎯 Answer:**

**Stateless JWT or shared Redis sessions.**

**JWT preferred.**

# **✅ Scenario 8**

### **👉 User changes password — what happens to tokens?**

### **🎯 Answer:**

**Invalidate all refresh tokens.**  
 **Force re-login everywhere.**

# **✅ Scenario 9**

### **👉 How do you implement RBAC in Node?**

### **🎯 Answer:**

* **Store roles in DB**

* **Add role to JWT**

* **Middleware checks permissions**

# **✅ Scenario 10 (System Design)**

### **👉 Design secure login system.**

### **🎯 Answer Outline:**

* **Password hashing (bcrypt)**

* **OAuth option**

* **JWT access token**

* **Refresh token in DB**

* **HTTPS**

* **Rate limiting**

* **RBAC**

* **Audit logs**

# **✅ Scenario 1**

### **👉 Your Node.js API becomes slow under high traffic. What do you do?**

### **🎯 Answer:**

1. **Identify bottleneck (CPU / DB / IO)**

2. **Add caching (Redis)**

3. **Use async/non-blocking code**

4. **Add DB indexes**

5. **Horizontal scaling (PM2 / containers)**

6. **Load balancer**

### **Interviewer testing:**

**Performance \+ Node event loop knowledge.**

# **✅ Scenario 2**

### **👉 Node server crashes frequently. How do you handle it?**

### **🎯 Answer:**

* **Use process manager (PM2)**

* **Graceful shutdown**

* **Centralized logging**

* **Health checks**

* **Auto restart**

* **Handle uncaught exceptions**

# **✅ Scenario 3**

### **👉 Design a highly available Node.js system.**

### **🎯 Answer:**

* **Multiple Node instances**

* **Load balancer (ALB / Nginx)**

* **Stateless APIs**

* **Redis / DB cluster**

* **Health checks**

* **Auto scaling**

# **✅ Scenario 4**

### **👉 How do you handle long-running tasks?**

### **🎯 Answer:**

* **Offload to background jobs**

* **Queue system (BullMQ / SQS)**

* **Worker processes**

**Never block event loop.**

 

# **✅ Scenario 5**

### **👉 Database goes down. What happens?**

### **🎯 Answer:**

* **Retry with backoff**

* **Circuit breaker**

* **Graceful error response**

* **Fallback cache**

* **Alerts**

# **✅ Scenario 6**

### **👉 Handle millions of concurrent users?**

### **🎯 Answer:**

* **Stateless JWT**

* **Horizontal scaling**

* **CDN**

* **Cache heavy reads**

* **Rate limiting**

* **Efficient DB queries**

# **✅ Scenario 7**

### **👉 Node.js is single-threaded. How does it scale?**

### **🎯 Answer:**

* **Event loop for async IO**

* **Cluster mode**

* **Worker threads for CPU tasks**

* **Multiple instances**

# **✅ Scenario 8**

### **👉 Prevent memory leaks?**

### **🎯 Answer:**

* **Avoid global variables**

* **Clean up listeners**

* **Close DB connections**

* **Heap snapshots**

* **Monitor memory**

# **✅ Scenario 9**

### **👉 API security design?**

### **🎯 Answer:**

* **HTTPS**

* **JWT auth**

* **RBAC**

* **Rate limiting**

* **Input validation**

* **Helmet**

* **CORS**

# **✅ Scenario 10**

### **👉 Zero-downtime deployment?**

### **🎯 Answer:**

* **Blue-green deployment**

* **Rolling updates**

* **Health checks**

* **Graceful shutdown**

# **✅ Scenario 12**

### **👉 Logging & monitoring strategy?**

### **🎯 Answer:**

* **Structured logs**

* **Centralized logging**

* **Metrics (CPU, memory, latency)**

* **Alerts**

# **⭐ System Design One-Liners (Memorize)**

* **“Node.js scales using async IO, not threads”**

* **“Stateless services scale horizontally”**

* **“Never block the event loop”**

* **“Cache what you read often”**

* **“Queues for slow work”**

## **module.exports vs exports**
- **What is the difference between `module.exports` and `exports`?**
   - `module.exports`: The actual object that gets returned by `require()`.
   - `exports`: A shorthand reference to `module.exports`. If you reassign `exports`, it breaks the link.
   - Best practice: Always use `module.exports`.

## **Handling Uncaught Exceptions**
- **How do you handle uncaught exceptions in Node.js?**
   - Use `process.on('uncaughtException')` for cleanup, but always restart the process using a manager like PM2.
   ```bash
   process.on('uncaughtException', (err) => {
     console.error('There was an uncaught error', err);
     process.exit(1); // mandatory (as per Node.js docs)
   });
   ```