---
layout: default
title: System Design
nav_order: 9
---

---
layout: default
title: System Design
nav_order: 9
---

# 🏗️ System Design Interview Questions

Principles of distributed systems, scalability, and high-level architectural patterns.

---

## 📑 Table of Contents
- [1️⃣ URL Shortener (e.g., Bitly)](#1️⃣-url-shortener-eg-bitly)
- [2️⃣ Real-Time Chat Application](#2️⃣-real-time-chat-application)
- [3️⃣ Notification System](#3️⃣-notification-system)
- [4️⃣ E-commerce System](#4️⃣-e-commerce-system)
- [5️⃣ Handling Slow APIs](#5️⃣-handling-slow-apis)
- [6️⃣ Monolith to Microservices](#6️⃣-monolith-to-microservices)
- [7️⃣ Apache Kafka](#7️⃣-apache-kafka)
- [8️⃣ RabbitMQ](#8️⃣-rabbitmq)
- [9️⃣ Kafka vs RabbitMQ](#9️⃣-kafka-vs-rabbitmq)
- [🔟 Redis Fundamentals](#🔟-redis-fundamentals)
- [1️⃣1️⃣ Database Scaling: Replication, Sharding, and Partitioning](#1️⃣1️⃣-database-scaling-replication-sharding-and-partitioning)
- [1️⃣2️⃣ The CAP Theorem](#1️⃣2️⃣-the-cap-theorem)
- [1️⃣3️⃣ Database vs Cache Consistency](#1️⃣3️⃣-database-vs-cache-consistency)

---

## **1️⃣ Design a URL Shortener (e.g., Bitly)**

**Answer:**
When designing a URL shortener, start by clarifying requirements like traffic volume. Assuming high traffic:

* **Main APIs**: One to create a short URL (`POST /create`) and one to redirect (`GET /:id`).
* **Storage**: A mapping of `short ID` -> `original URL` in a persistent store (PostgreSQL) or a key-value store.
* **Generation**: Use **Base62 encoding** of an auto-increment ID or a distributed ID generator (like Snowflake) to avoid collisions.
* **Caching**: Place **Redis** in front for fast redirects (reads are much higher than writes).
* **Scalability**: Use load balancers and database sharding if needed.
* **Analytics**: Process click tracking asynchronously using a queue (e.g., Kafka) to avoid slowing down the redirect flow.

---

## **2️⃣ Design a Real-Time Chat Application**

**Answer:**
A real-time chat system requires low latency and high concurrency:

* **Communication**: Use **WebSockets** for real-time bidirectional communication instead of polling.
* **Storage**: Use a flexible database like **MongoDB** or a specialized NoSQL store for message history.
* **Scalability**: Introduce **Kafka** or **Redis Pub/Sub** to sync messages across multiple backend servers.
* **Offline Handling**: Store messages in the database and deliver them when the user reconnects.
* **Ordering**: Use timestamps or sequence IDs to ensure messages appear in the correct order.

---

## **3️⃣ Design a Notification System**

**Answer:**
A robust notification system should be decoupled from the main application:

* **Architecture**: The main app pushes events into a **message queue**.
* **Workers**: Specialized worker services consume events and send notifications via different channels (Email, Push, SMS).
* **Reliability**: Implement **retries with exponential backoff** and a **Dead-Letter Queue (DLQ)** for failed deliveries.
* **Preferences**: Store and respect user configuration for notification types and frequencies.

---

## **4️⃣ Design an E-commerce System**

**Answer:**
An e-commerce backend must handle complex transactions and consistency:

* **Microservices**: Split into services like Product, Cart, Order, and Payment.
* **Inventory Consistency**: Use **atomic updates** or **distributed locking** to prevent overselling items.
* **Transactions**: Use the **Saga pattern** to ensure reliability across microservices.
* **Performance**: Cache product listings and use a **CDN** for static assets and images.

**What is the Saga Pattern?**
In a microservices system, you can't use a single database transaction across multiple services. The Saga pattern breaks a multi-step business transaction into a sequence of local transactions, each publishing an event for the next step. If one step fails, compensating transactions undo the previous steps.

```
Order Saga — "Place an order" across 3 services:

Step 1: Order Service    → creates order (status: pending)
         ↓ success event
Step 2: Inventory Service → reserves items
         ↓ success event
Step 3: Payment Service  → charges card
         ↓ success event
        Order confirmed!

If Payment fails:
Step 3 FAILS → Payment publishes "payment-failed" event
Step 2 runs COMPENSATING transaction → releases reserved items
Step 1 runs COMPENSATING transaction → cancels order
```

---

## **5️⃣ How do you handle a slow API in production?**

**Answer:**
1. **Identify Bottlenecks**: Use monitoring tools (APM) and logs to trace whether the issue is in the DB, network, or app logic.
2. **Database Optimization**: Check for missing indexes or inefficient queries. Use execution plans (**EXPLAIN**).
3. **Caching**: Introduce **Redis** for frequently accessed, slow-changing data.
4. **Horizontal Scaling**: Scale the application instances behind a load balancer.
5. **Data-Driven Approach**: Always use metrics and tracing to guide optimizations.

---

## **6️⃣ How would you migrate a Monolith to Microservices?**

**Answer:**
Migration should be gradual to minimize risk:
* **Strangler Fig Pattern**: Gradually extract features into separate services instead of a "big bang" rewrite. Named after a fig tree that slowly grows around and replaces a host tree — your new microservices wrap the monolith and take over one feature at a time until the monolith is retired.
* **Start Small**: Begin with less critical modules like notifications or search.
* **Define Boundaries**: Establish clear service boundaries and use API-based communication.
* **Gradual Transition**: Maintain the monolith while slowly redirecting traffic to new services.

---

## **7️⃣ What is Apache Kafka and its core components?**

**Answer:**
**Apache Kafka** is a distributed event streaming platform used for high-performance data pipelines.
* **Core Idea**: Producers send messages -> Kafka stores them -> Consumers read them.
* **Components**:
    * **Producer**: Sends data to Kafka.
    * **Consumer**: Reads data from Kafka.
    * **Topic**: A category or channel for messages.
    * **Partition**: Topics are split into partitions for parallel processing.
    * **Broker**: A Kafka server that stores and serves data.
    * **Consumer Group**: A group of consumers working together to process data.

**How a message flows through Kafka (simple example):**
```
Order placed on website
       ↓
Producer (Order Service) → publishes to Topic: "order-events"
                                    ↓
                    Kafka stores it in a Partition (durable log)
                                    ↓
Consumer Group A (Inventory Service) ← reads & reserves stock
Consumer Group B (Email Service)     ← reads & sends confirmation email
Consumer Group C (Analytics)         ← reads & updates dashboard

Key point: Both services read the SAME message independently.
Kafka doesn't delete the message after one consumer reads it.
```

**Why partitions?** Each partition can be consumed by one consumer in a group — so with 3 partitions you can run 3 consumers in parallel, tripling throughput.

---

## **8️⃣ What is RabbitMQ and how does it work?**

**Answer:**
**RabbitMQ** is a message broker that supports complex routing logic.
* **Core Idea**: Producers -> **Exchange** -> **Queues** (based on **Bindings**) -> Consumers.
* **Exchange Types**:
    * **Direct**: Exact routing key match. *(Use for: specific task routing — "process payment" only goes to payment queue)*
    * **Fanout**: Broadcasts to all bound queues. *(Use for: notifications to multiple services — every service gets a copy)*
    * **Topic**: Wildcard patterns in routing keys. *(Use for: "send to all EU-related queues" — `eu.*` matches eu.orders, eu.users)*
    * **Headers**: Uses message headers for routing. *(Rarely used — complex, prefer Topic)*

---

## **9️⃣ Kafka vs RabbitMQ: When to use which?**

| Feature | Apache Kafka | RabbitMQ |
| :--- | :--- | :--- |
| **Model** | Pull-based (Log) | Push-based (Smart Broker) |
| **Persistence** | Durable (Retains messages) | Transient (Deletes after ack) |
| **Throughput** | Extremely High | High |
| **Best For** | Event streaming, Analytics | Task queues, Microservice communication |

**Pull vs Push — what does it mean?**
* **Kafka (Pull):** Consumers ask Kafka "give me the next message." Consumers control their own pace — a slow consumer just reads slower, it doesn't crash Kafka.
* **RabbitMQ (Push):** RabbitMQ pushes messages to consumers as fast as they arrive. Faster delivery, but a slow consumer can get overwhelmed and queue up messages.

**Simple rule:** Use **Kafka** when you need event history, high throughput, or multiple independent consumers reading the same events. Use **RabbitMQ** when you need reliable task queues with complex routing and message acknowledgement.

---

## **🔟 What is Redis and why is it so fast?**

**Answer:**
Redis is an in-memory data structure store used as a database, cache, and message broker.

### **Why is it fast?**
* **In-Memory**: Operates entirely in RAM.
* **Single-Threaded**: Eliminates context switching and locking overhead.
* **Simple Data Structures**: Highly optimized code.
* **Written in C**: High-performance implementation.

> [!TIP]
> **Interview Gold:** When designing any system, always address **Single Points of Failure (SPOF)** by introducing redundancy and health checks.

---

## **1️⃣1️⃣ Database Scaling: Replication, Sharding, and Partitioning**

**Question:** "Our database is becoming a bottleneck. How would you scale it to handle more traffic and data?"

**Simple Explanation (The Library Analogy):**
Imagine a library that is getting too crowded:

1.  **Vertical Scaling (Bigger Server):** You buy a bigger building and hire a faster librarian. It's the simplest fix but has a hard limit—eventually, you can't buy a bigger building.
2.  **Horizontal Scaling (More Servers):** Instead of one giant building, you decide to use **multiple buildings**. This is how you scale "out" instead of "up." To do this for a database, we use two main methods:
    *   **Replication (Read Replicas):** You make photocopies of the books and put them in other buildings. People can read the copies (Read), but if you want to add a new book (Write), you must update the master and wait for it to sync with the copies. **Best for read-heavy apps.**
    *   **Sharding (Data Splitting):** You open different library branches. You put books A-M in Branch 1 and N-Z in Branch 2. Each building has its own data. **Best for scaling both reads and writes when data volume is massive.**

**Related Questions:**
- **"What is the biggest challenge with Sharding?"** Answer: Complexity. Joining data across shards is very slow, and re-balancing data (re-sharding) is a nightmare.
- **"When should you NOT shard?"** Answer: When you can still scale vertically or if your application requires complex joins across the data you are splitting.

---

## **1️⃣2️⃣ The CAP Theorem**

**Question:** "What is the CAP theorem and why does it matter?"

**Simple Explanation (The ATM Analogy):**
CAP stands for **C**onsistency, **A**vailability, and **P**artition Tolerance. The rule is: *In a distributed system, you can only have 2 out of 3.*

Imagine an ATM (Distributed Node) trying to talk to the Bank (The Network):
1.  **Consistency (C):** Every time you check your balance, it's 100% correct across all ATMs.
2.  **Availability (A):** The ATM always gives you money, even if it can't talk to the bank.
3.  **Partition Tolerance (P):** The system keeps working even if the connection between the ATM and the Bank is broken.

**The Trade-off:** If the connection breaks (Partition), you must choose:
-   **CP (Consistency over Availability):** The ATM refuses to give you money because it doesn't know your exact balance. (e.g., Banking systems).
-   **AP (Availability over Consistency):** The ATM gives you money anyway, but it might be wrong. You'll fix it later when the connection returns. (e.g., Social media likes).

---

## **1️⃣3️⃣ Database vs Cache Consistency**

**Question:** "How do you ensure your Redis cache doesn't have old data compared to your DB?"

**Simple Explanation:**
1.  **Cache-Aside (Most Common):** The app checks the cache. If it's a "miss," it reads from the DB and writes to the cache. To keep it fresh, you **delete** the cache entry whenever you update the DB.
2.  **Write-Through:** The app writes to the cache, and the cache *immediately* updates the DB. Data is never stale, but writes are slower.
3.  **Write-Back:** The app writes only to the cache. The cache updates the DB later in a batch. Extremely fast, but if the cache crashes before the update, you lose data.

**Related Question:** "Why should you DELETE the cache instead of UPDATING it on a DB write?"
**Answer:** To avoid race conditions. If two updates happen at once, the order in the cache might end up different from the DB. Deleting it forces a fresh, correct read next time.

> [!TIP]
> **Interview Gold:** For mid-level roles, always mention **"Eventual Consistency"**. Most modern systems don't need absolute consistency; it's okay if a "Like" count takes a few seconds to update across the globe.

---

## **1️⃣4️⃣ Rate Limiting Algorithms**

Rate limiting controls how many requests a client can make in a time window to protect your system from abuse.

### **Fixed Window**
* Allow N requests per minute. Counter resets at the top of every minute.
* **Problem:** A user can send N requests at 11:59 and N more at 12:00 — double the limit at the boundary.

### **Sliding Window**
* Tracks requests over a rolling window (last 60 seconds, not "this minute").
* Smoother — no boundary spikes. Slightly more memory.

### **Token Bucket** *(most popular)*
* Each user has a bucket holding up to N tokens. Each request costs 1 token. Tokens refill at a steady rate.
* Allows **short bursts** (empty the bucket fast) but enforces a long-term average rate.

### **Leaky Bucket**
* Requests go into a queue (bucket). The queue is drained at a fixed rate.
* Smooths out bursts completely — output rate is always constant.

> [!TIP]
> **Interview Gold:** Token Bucket is the most common choice because it allows natural bursts (a user opening your app) while still enforcing limits. Redis is the standard storage for distributed rate limiting counters.

---

## **1️⃣5️⃣ Circuit Breaker Pattern**

**What it is:** A pattern that stops calling a failing service and returns a fallback immediately, preventing a slow or broken dependency from cascading failure through your whole system.

### **Three states**
1. **Closed (normal):** Requests pass through. If failures exceed a threshold → trips to Open.
2. **Open (tripped):** All requests fail immediately with a fallback. No real calls made. After a timeout → moves to Half-Open.
3. **Half-Open (testing):** Lets a few requests through. If they succeed → back to Closed. If they fail → back to Open.

### **Simple analogy**
Think of it like an electrical circuit breaker. When too much current flows (too many failures), the breaker trips (Open state) to protect the circuit. After a cooldown, it resets and tries again.

```
Normal → Failure threshold hit → Circuit OPEN (return fallback immediately)
     ↑                                      ↓ (after timeout)
     ←──── Successes ────── Half-Open (test a few requests)
```

> [!TIP]
> **Interview Gold:** Without a circuit breaker, one slow microservice can cause all your other services to pile up waiting, eventually crashing the entire system. The circuit breaker is the "fail fast, recover gracefully" pattern.

---

## **1️⃣6️⃣ Consistent Hashing**

**The problem:** In a standard cache cluster with N servers, you use `hash(key) % N` to decide which server stores a key. If you add or remove a server, N changes — and almost **every key remaps to a different server**, causing a massive cache miss storm.

**Consistent Hashing solution:**
* Place both servers and keys on a virtual ring (0 to 2³² − 1).
* A key is stored on the **first server clockwise** from its position on the ring.
* When a server is added/removed, only the keys on the adjacent arc need to move — roughly `1/N` of all keys instead of almost all of them.

```
Ring:  [Server A]---[Key "user:1"]---[Server B]---[Key "user:2"]---[Server C]

"user:1" maps to Server B (first server clockwise)
If Server B is removed, "user:1" remaps to Server C — other keys unaffected.
```

### **Virtual nodes**
Real systems give each server multiple positions (virtual nodes) on the ring for more even load distribution.

> [!TIP]
> **Interview Gold:** Consistent hashing is used in **distributed caches** (Redis Cluster, Memcached), **CDNs**, and **database sharding** (Cassandra, DynamoDB). Mention it when asked "how do you add servers to a cache cluster without a thundering herd?"
