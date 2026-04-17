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

---

## **7️⃣ What is Lambda?**

**AWS Lambda** lets you **run code without managing servers**.

This is called **serverless computing**.

> [!TIP]
> **Example:** Image upload → trigger Lambda → resize image → store in S3.

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
