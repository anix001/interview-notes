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
* **Strangler Fig Pattern**: Gradually extract features into separate services instead of a "big bang" rewrite.
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

---

## **8️⃣ What is RabbitMQ and how does it work?**

**Answer:**
**RabbitMQ** is a message broker that supports complex routing logic.
* **Core Idea**: Producers -> **Exchange** -> **Queues** (based on **Bindings**) -> Consumers.
* **Exchange Types**:
    * **Direct**: Exact routing key match.
    * **Fanout**: Broadcasts to all bound queues.
    * **Topic**: Wildcard patterns in routing keys.
    * **Headers**: Uses message headers for routing.

---

## **9️⃣ Kafka vs RabbitMQ: When to use which?**

| Feature | Apache Kafka | RabbitMQ |
| :--- | :--- | :--- |
| **Model** | Pull-based (Log) | Push-based (Smart Broker) |
| **Persistence** | Durable (Retains messages) | Transient (Deletes after ack) |
| **Throughput** | Extremely High | High |
| **Best For** | Event streaming, Analytics | Task queues, Microservice communication |

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
