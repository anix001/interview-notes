---
layout: default
title: AWS
nav_order: 7
---

# ☁️ AWS Interview Questions

Amazon Web Services (AWS) concepts, core services, and infrastructure basics.

---

## 📑 Table of Contents
- [1️⃣ What is AWS?](#1️⃣-what-is-aws)
- [2️⃣ What is EC2?](#2️⃣-what-is-ec2)
- [3️⃣ What is S3?](#3️⃣-what-is-s3)
- [4️⃣ What is IAM?](#4️⃣-what-is-iam)
- [5️⃣ What is a VPC?](#5️⃣-what-is-a-vpc)
- [6️⃣ What is RDS?](#6️⃣-what-is-rds)
- [7️⃣ What is Lambda?](#7️⃣-what-is-lambda)
- [8️⃣ What is CloudFront?](#8️⃣-what-is-cloudfront)
- [Edge Location](#edge-location)
- [Load Balancer vs API Gateway?](#load-balancer-vs-api-gateway)
- [Horizontal vs Vertical Scaling?](#horizontal-vs-vertical-scaling)

---

## **1️⃣ What is AWS?**

**Answer:**

**Amazon Web Services (AWS)** is a **cloud computing platform** that provides services like servers, storage, databases, networking, and AI over the internet.

Instead of buying physical servers, you can **rent infrastructure on demand**.

### Example services:
* **Compute** → Amazon EC2
* **Storage** → Amazon S3
* **Database** → Amazon RDS

---

## **2️⃣ What is EC2?**

**Amazon EC2 (Elastic Compute Cloud)** is a service that provides **virtual servers in the cloud**.

You use it to:
* host applications
* run backend servers
* deploy APIs

> [!NOTE]
> **Example:** Running a **Node.js backend on an EC2 instance**.

---

## **3️⃣ What is S3?**

**Amazon S3 (Simple Storage Service)** is an **object storage service** used to store files.

Used for:
* images
* videos
* backups
* static websites

> [!NOTE]
> **Example:** Uploading user profile images to an **S3 bucket**.

---

## **4️⃣ What is IAM?**

**AWS Identity and Access Management (IAM)** is used to **control access to AWS resources**.

You can create:
* **Users**
* **Roles**
* **Policies**

> [!TIP]
> **Example:**
> * Developer → EC2 access
> * Admin → full access

---

## **5️⃣ What is a VPC?**

**Amazon Virtual Private Cloud (VPC)** is a **private network inside AWS**.

You can:
* define IP ranges
* create public/private subnets
* control security

Think of it like **your own private data center in the cloud**.

---

## **6️⃣ What is RDS?**

**Amazon RDS (Relational Database Service)** is a **managed relational database service**.

### Supported databases:
* MySQL
* PostgreSQL
* MariaDB
* Oracle
* SQL Server

### Benefits:
* automatic backups
* scaling
* maintenance handled by AWS

**What "managed" actually means — AWS does all of this for you:**

| Task | Without RDS (self-managed) | With RDS |
|------|---------------------------|----------|
| OS patches | You SSH in and update | AWS handles it |
| Database backups | You write cron jobs | Automatic daily snapshots |
| Failover | You set up replication manually | Multi-AZ failover in ~60 seconds |
| Scaling storage | You resize the disk manually | Auto Storage Scaling |
| Read replicas | Complex setup | 1-click in the console |

> [!TIP]
> **Interview Gold:** RDS is worth paying the premium for because it eliminates the hardest DBA work — backups, failover, and patching. You focus on your app, not your database infrastructure.

---

## **7️⃣ What is Lambda?**

**AWS Lambda** lets you **run code without managing servers**.

This is called **serverless computing**.

> [!TIP]
> **Example:** Image upload → trigger Lambda → resize image → store in S3.

**How it works:**
* You write a function. AWS runs it on demand.
* You pay only for the time your function actually runs (per 1ms of execution).
* AWS handles provisioning, scaling, and OS — you never SSH into anything.

**Cold starts** — the only real downside:
* When a Lambda hasn't been called in a while, AWS "freezes" the container.
* The first call after a freeze has extra latency (~100ms–1s) while the container wakes up.
* Subsequent calls are fast (the container is "warm").

**When NOT to use Lambda:**
* Long-running tasks (> 15 minute timeout limit).
* Workloads that need persistent connections (WebSockets with state, large DB connection pools).
* CPU-heavy processing that runs continuously — EC2 is cheaper for 24/7 workloads.

> [!TIP]
> **Interview Gold:** Lambda is perfect for event-driven, short-lived tasks — file processing, webhooks, scheduled jobs, API endpoints with variable traffic. Use EC2 or ECS for always-on servers with predictable load.

---

## **8️⃣ What is CloudFront?**

**Amazon CloudFront** is a **Content Delivery Network (CDN)**.

It speeds up websites by delivering content from **servers closer to the user**.

> [!NOTE]
> **Example:** Serving static files globally.

---

## **Edge Location**
- **What is an Edge Location?**
   - Small data centers used by **CloudFront** (CDN) to cache your content closer to the users, reducing latency.

**How caching at an Edge Location works:**

```
First request (cache MISS):
User in Mumbai → Mumbai Edge Location → "I don't have this file"
                                      → fetches from your S3 in US-East
                                      → caches it locally → returns to user
                                      (slow — had to go all the way to US)

Second request (cache HIT):
User in Mumbai → Mumbai Edge Location → "I have this file!"
                                      → returns immediately from cache
                                      (fast — served from Mumbai, not US)
```

* The file stays cached until its TTL (Time To Live) expires or you invalidate it.
* CloudFront has 400+ edge locations globally — users always hit the nearest one.

> [!TIP]
> **Interview Gold:** CDNs like CloudFront reduce latency by moving content physically closer to users. They also reduce load on your origin server — 90% of requests never reach your backend at all.

---

## **Load Balancer vs API Gateway?**

**Answer:**

* **Load Balancer (LB)**:
    * **Purpose**: Distributes incoming network traffic across multiple backend servers to ensure no single server is overloaded.
    * **Focus**: Availability and reliability at the network/transport layer (L4 or L7).
* **API Gateway**:
    * **Purpose**: A single entry point for all clients. It sits in front of microservices.
    * **Focus**: High-level concerns like Authentication, Authorization, Rate Limiting, Request Transformation, and Protocol Translation (e.g., REST to gRPC).

> [!IMPORTANT]
> **Key Difference**: A Load Balancer just routes traffic; an API Gateway "manages" the API requests with business logic (security, logging, etc.).

---

## **Horizontal vs Vertical Scaling?**

**Answer:**

* **Vertical Scaling (Scale Up)**:
    * Adding more power (CPU, RAM) to an existing server.
    * **Limit**: Eventually, you hit the hardware ceiling of a single machine.
* **Horizontal Scaling (Scale Out)**:
    * Adding more servers to the resource pool.
    * **Benefit**: Practically limitless scaling and better fault tolerance.
    * **Requirement**: Requires a Load Balancer and stateless application design.

---

## **9️⃣ What is DynamoDB?**

**Amazon DynamoDB** is AWS's fully managed **NoSQL database** — serverless, infinitely scalable, and extremely fast (single-digit millisecond responses).

* **Key-Value & Document:** Stores data as items (rows) with attributes (columns). No fixed schema.
* **Primary Key:** Every item must have a partition key (and optionally a sort key).
* **Auto-scaling:** Scales up and down automatically based on traffic. You pay per request.
* **Use cases:** Shopping carts, user sessions, leaderboards, IoT events — anything that needs speed at massive scale.

```
Table: Users
Partition Key: userId (string)
Sort Key:      createdAt (number)

Item: { userId: "u1", createdAt: 1700000000, name: "Alice", role: "admin" }
```

> [!TIP]
> **Interview Gold:** DynamoDB is not for complex joins or reporting. Choose it when you need **massive scale and low latency**. Design your access patterns first — unlike SQL, you model the table around how you'll query it.

---

## **🔟 What is ElastiCache?**

**Amazon ElastiCache** is a managed **in-memory caching** service. It supports two engines:
* **Redis** — most common. Supports strings, lists, sets, sorted sets, pub/sub, and persistence.
* **Memcached** — simpler, multi-threaded, pure caching only.

### **Why use it?**
* Reduce database load by caching expensive query results.
* Store user sessions so any server can read them (stateless architecture).
* Power real-time leaderboards and rate limiting counters.

### **Common pattern (Cache-Aside)**
1. App checks Redis first.
2. Cache hit → return immediately.
3. Cache miss → query DB, store result in Redis, return.

> [!TIP]
> **Interview Gold:** Redis on ElastiCache is the go-to answer whenever an interviewer asks "how would you speed up a slow database query?" or "how would you handle sessions across multiple servers?"

---

## **1️⃣1️⃣ What is CloudWatch?**

**Amazon CloudWatch** is AWS's monitoring and observability service. Think of it as the "eyes" of your AWS infrastructure.

* **Metrics:** Collects data like CPU usage, memory, request count, error rates from AWS services.
* **Logs:** Centralized log storage from Lambda, EC2, ECS, etc. Searchable with CloudWatch Logs Insights.
* **Alarms:** Trigger notifications (or auto-scaling) when a metric crosses a threshold (e.g., CPU > 80%).
* **Dashboards:** Visual charts for your metrics.

### **Common use**
```
Lambda crashes → sends logs to CloudWatch Logs
CloudWatch sees Error count > 10 in 5 min → triggers SNS alarm → sends email
```

> [!TIP]
> **Interview Gold:** Every production app on AWS should have CloudWatch alarms for error rates and latency. If you can't observe it, you can't debug it.

---

## **1️⃣2️⃣ What is SQS & SNS?**

These are AWS messaging services for **decoupling** components of your system.

### **SQS (Simple Queue Service) — Queue**
* Messages sit in a queue until a consumer picks them up and processes them.
* One message → one consumer (point-to-point).
* Use when: you want to process tasks in the background (send email, resize image, generate PDF).

### **SNS (Simple Notification Service) — Pub/Sub**
* One message → **many subscribers** (broadcast).
* Subscribers can be SQS queues, Lambda functions, email addresses, HTTP endpoints.
* Use when: you need to notify multiple services about the same event (user signed up → send welcome email + trigger analytics + update CRM).

```
User places order
   → SNS topic "order-placed"
       → SQS queue → inventory service
       → SQS queue → email service
       → Lambda → analytics
```

> [!TIP]
> **Interview Gold:** SQS + SNS together is the classic "fan-out" pattern. SNS broadcasts the event; each SQS queue buffers it so each service processes at its own pace without being overwhelmed.
