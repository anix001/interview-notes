---
layout: default
title: System Design
nav_order: 9
---

# 🏗️ System Design Interview Questions

Principles of distributed systems, scalability, and high-level architectural patterns.

---

## 📑 Table of Contents
- [Case Study: URL Shortener](#-case-study-url-shortener)
- [Real-Time Systems: Chat App](#-real-time-systems-chat-app)
- [Scalability & Migration](#-scalability--migration)
- [Message Queues: Kafka vs RabbitMQ](#-message-queues-kafka-vs-rabbitmq)
- [In-Memory Stores: Redis](#-in-memory-stores-redis)

---

## 🔗 Case Study: URL Shortener

Designing a system like Bitly requires handling high traffic and fast redirects.
* **Storage:** PostgreSQL for persistent mapping or Redis for speed.
* **Generation:** Use **Base62 encoding** on an auto-incrementing ID.
* **Caching:** Use Redis to store the most frequent `ShortID -> OriginalURL` mappings.
* **Analytics:** Process click tracking asynchronously using a queue (Kafka).

---

## 💬 Real-Time Systems: Chat App

Key requirements: Low latency and high concurrency.
* **Communication:** Use **WebSockets** for bidirectional real-time communication.
* **Scalability:** Use a **Pub/Sub** mechanism (Redis or Kafka) to sync messages across multiple server instances.
* **Ordering:** Use sequence IDs or timestamps to maintain message order.

---

## 🚀 Scalability & Migration

### Monolith to Microservices
* **Strangler Fig Pattern:** Gradually replace parts of the monolith with new microservices rather than a full rewrite.
* **Boundary Definition:** Identify distinct business domains (e.g., Auth, Payments, Inventory).

### Handling Slow APIs
1. **Identify:** Use APM tools to find the bottleneck (DB, Network, CPU).
2. **Optimize:** Fix inefficient queries or add missing indexes.
3. **Cache:** Use Redis for slow-changing data.
4. **Scale:** Add more instances behind a Load Balancer.

---

## 📩 Message Queues: Kafka vs RabbitMQ

| Feature | Apache Kafka | RabbitMQ |
| :--- | :--- | :--- |
| **Model** | Pull-based (Log) | Push-based (Smart Broker) |
| **Throughput** | Extremely High | High |
| **Persistence** | Durable (Retains messages) | Transient (Deletes after ack) |
| **Best For** | Event streaming, Analytics | Task queues, Microservice communication |

---

## ⚡ In-Memory Stores: Redis

Redis is extremely fast because it operates entirely in RAM and uses a single-threaded execution model to avoid locking overhead.

### Common Use Cases:
* **Caching:** Speeding up API responses.
* **Session Store:** Managing user logins.
* **Rate Limiting:** Protecting APIs from abuse.
* **Leaderboards:** Using `Sorted Sets` for real-time rankings.

> [!IMPORTANT]
> **Single-Threaded Model:** Redis' speed comes from its simple data structures and the fact that it doesn't waste time on context switching between threads.

---

## 🛠️ Reliability Patterns

### Circuit Breaker
Prevents a system from repeatedly trying to perform an operation that's likely to fail, allowing it to recover gracefully.

### Dead-Letter Queue (DLQ)
A specialized queue for messages that cannot be processed successfully after multiple retries.

> [!TIP]
> **Interview Gold:** When designing any system, always address **Single Points of Failure (SPOF)** by introducing redundancy and health checks.
