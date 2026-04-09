# 🏗️ System Design Interview Questions

## **1️⃣ Design a URL Shortener (e.g., Bitly)**

**Answer:**

When designing a URL shortener like Bitly, start by clarifying requirements like traffic volume. Assuming high traffic (millions of redirects per day):

* **Main APIs**: One to create a short URL and one to redirect.
* **Storage**: A mapping of `short ID` -> `original URL` in a persistent store (PostgreSQL) or a key-value store.
* **Generation**: Use **Base62 encoding** of an auto-increment ID or a distributed ID generator to avoid collisions.
* **Caching**: Place **Redis** in front for fast redirects (reads are much higher than writes).
* **Scalability**: Use load balancers and database sharding if needed.
* **Analytics**: Process click tracking asynchronously using a queue (e.g., Kafka) to avoid slowing down the redirect flow.

## **2️⃣ Design a Real-Time Chat Application**

**Answer:**

A real-time chat system requires low latency and high concurrency:

* **Communication**: Use **WebSockets** for real-time bidirectional communication instead of polling.
* **Storage**: Use a flexible database like **MongoDB** or a specialized NoSQL store for message history.
* **Scalability**: Introduce **Kafka** or **Redis Pub/Sub** to sync messages across multiple backend servers.
* **Offline Handling**: Store messages in the database and deliver them when the user reconnects.
* **Ordering**: Use timestamps or sequence IDs to ensure messages appear in the correct order.

## **3️⃣ Design a Notification System**

**Answer:**

A robust notification system should be decoupled from the main application:

* **Architecture**: The main app pushes events into a **message queue**.
* **Workers**: Specialized worker services consume events and send notifications via different channels (Email, Push, SMS).
* **Reliability**: Implement **retries with exponential backoff** and a **Dead-Letter Queue (DLQ)** for failed deliveries.
* **Preferences**: Store and respect user configuration for notification types and frequencies.

## **4️⃣ Design an E-commerce System**

**Answer:**

An e-commerce backend must handle complex transactions and consistency:

* **Microservices**: Split into services like Product, Cart, Order, and Payment.
* **Inventory Consistency**: Use **atomic updates** or **distributed locking** to prevent overselling items.
* **Transactions**: Use the **Saga pattern** or a transactional approach to ensure reliability across services.
* **Performance**: Cache product listings and use a **CDN** for static assets and images.

## **5️⃣ How do you handle a slow API in production?**

**Answer:**

To troubleshoot and fix a slow API:

1. **Identify Bottlenecks**: Use monitoring tools (APM) and logs to trace whether the issue is in the DB, network, or app logic.
2. **Database Optimization**: Check for missing indexes or inefficient queries. Use execution plans (EXPLAIN).
3. **Caching**: Introduce **Redis** for frequently accessed, slow-changing data.
4. **Horizontal Scaling**: Scale the application instances behind a load balancer.
5. **Data-Driven Approach**: Always use metrics and tracing to guide optimizations.

## **6️⃣ How would you migrate a Monolith to Microservices?**

**Answer:**

Migration should be gradual to minimize risk:

* **Strangler Fig Pattern**: Gradually extract features into separate services instead of a "big bang" rewrite.
* **Start Small**: Begin with less critical modules like notifications or search.
* **Define Boundaries**: Establish clear service boundaries and use API-based communication.
* **Gradual Transition**: Maintain the monolith while slowly redirecting traffic to new services until the monolith is decommissioned.

## **7️⃣ What is Apache Kafka and its core components?**

**Answer:**

**Apache Kafka** is a distributed event streaming platform used for high-performance data pipelines and real-time analytics.

* **Core Idea**: Producers send messages -> Kafka stores them -> Consumers read them.
* **Components**:
    * **Producer**: The service that sends data to Kafka.
    * **Consumer**: The service that reads data from Kafka.
    * **Topic**: A category or channel for messages (e.g., `user-events`).
    * **Partition**: Topics are split into partitions for parallel processing and scaling.
    * **Broker**: A Kafka server that stores and serves data.
    * **Offset**: A unique ID for each message within a partition, used to track consumption.
    * **Consumer Group**: A group of consumers working together to process data in parallel.

**Example Flow (E-commerce)**:
1. User places order -> Backend (Producer) sends event to `order-events` topic.
2. Kafka stores the event.
3. Multiple Consumers (Email service, Analytics, Shipping) independently read and process the event.

## **8️⃣ What is RabbitMQ and how does it work?**

**Answer:**

**RabbitMQ** is a popular open-source message broker that supports multiple messaging protocols. It is designed to handle complex routing logic and ensure reliable delivery.

* **Core Idea**: Producers send messages to an **Exchange** -> Exchange routes messages to **Queues** based on rules (Bindings) -> Consumers read from Queues.
* **Components**:
    * **Producer**: Sends messages.
    * **Exchange**: The "brains" that receives messages and decides which queue to send them to.
    * **Queue**: A buffer that stores messages until they are consumed.
    * **Binding**: A link between an exchange and a queue.
    * **Consumer**: Processes messages from the queue.
* **Exchange Types**:
    * **Direct**: Routes messages based on an exact routing key match.
    * **Fanout**: Broadcasts messages to all bound queues (ignores routing keys).
    * **Topic**: Routes messages based on wildcard patterns in routing keys (e.g., `user.*`).
    * **Headers**: Uses message headers instead of routing keys for routing.

## **9️⃣ Kafka vs RabbitMQ: When to use which?**

**Answer:**

| Feature | Apache Kafka | RabbitMQ |
| :--- | :--- | :--- |
| **Model** | Pull-based (Consumers pull data) | Push-based (Broker pushes to consumers) |
| **Durable?** | Yes (Stores messages as a log) | Transient (Deletes after Ack by default) |
| **Throughput** | Extremely High (Millions of msg/sec) | High (Thousands of msg/sec) |
| **Routing** | Simple (Topic-based) | Complex (Flexible Exchange types) |
| **Best Use Case** | Log aggregation, Stream processing | Microservice communication, Task queues |

**Summary**: Use **Kafka** for high-volume data streaming and event history. Use **RabbitMQ** for complex routing requirements and reliable task distribution.

## **🔟 What is Redis and why is it so fast?**

**Answer:**

**Redis** (Remote Dictionary Server) is an open-source, in-memory data structure store used as a database, cache, message broker, and streaming engine.

* **Why is it fast?**
    * **In-Memory**: Operates entirely in RAM, avoiding slow disk I/O.
    * **Single-Threaded**: Uses a single execution thread, which eliminates overhead from context switching and locking.
    * **Simple Data Structures**: Optimized code for strings, hashes, lists, sets, etc.
    * **Written in C**: High-performance, low-level language implementation.
* **Common Use Cases**:
    * **Caching**: Storing DB query results to speed up APIs.
    * **Session Management**: Storing user login sessions.
    * **Rate Limiting**: Counting API requests per user to prevent abuse.
    * **Pub/Sub**: Real-time messaging/notifications.
    * **Leaderboards**: Using `Sorted Sets` to maintain rankings.

