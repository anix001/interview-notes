---
layout: default
title: AWS
nav_order: 6
---

# ☁️ AWS Interview Questions

Key concepts of Amazon Web Services, cloud architecture, and infrastructure management.

---

## 📑 Table of Contents
- [Core Services (EC2, S3, RDS)](#-core-services-ec2-s3-rds)
- [IAM & Security](#-iam--security)
- [Networking: VPC](#-networking-vpc)
- [Serverless: Lambda](#-serverless-lambda)
- [Content Delivery: CloudFront](#-content-delivery-cloudfront)
- [Scaling & Traffic Management](#-scaling--traffic-management)

---

## 🏗️ Core Services (EC2, S3, RDS)

### 1️⃣ Amazon EC2 (Elastic Compute Cloud)
Provides **virtual servers** in the cloud. Used to host applications, run backends, and deploy APIs.

### 2️⃣ Amazon S3 (Simple Storage Service)
An **object storage** service used for storing files such as images, videos, backups, and static website assets.

### 3️⃣ Amazon RDS (Relational Database Service)
A **managed database service** supporting MySQL, PostgreSQL, MariaDB, and more. It handles backups, patching, and scaling automatically.

---

## 🔐 IAM & Security

**AWS IAM (Identity and Access Management)** controls access to AWS resources.
* **Users:** Individuals with long-term credentials.
* **Roles:** Temporary permissions for services or users.
* **Policies:** JSON documents defining what actions are allowed on which resources.

---

## 🌐 Networking: VPC

**Amazon VPC (Virtual Private Cloud)** is your own private network within AWS.
* **Subnets:** Public (accessible from internet) and Private (isolated).
* **Security Groups:** Act as a virtual firewall for your EC2 instances.

---

## ⚡ Serverless: Lambda

**AWS Lambda** allows you to run code without managing servers (Serverless). You only pay for the compute time you consume.
* *Example:* Image upload to S3 triggers a Lambda function to generate thumbnails.

---

## 🚀 Content Delivery: CloudFront

**Amazon CloudFront** is a **CDN (Content Delivery Network)** that speeds up the delivery of content by caching it at **Edge Locations** closer to users.

---

## 📈 Scaling & Traffic Management

### Horizontal vs Vertical Scaling
* **Vertical (Scale Up):** Adding more CPU/RAM to an existing server. Has a hardware ceiling.
* **Horizontal (Scale Out):** Adding more servers. Practically limitless and provides better fault tolerance.

### Load Balancer vs API Gateway
* **Load Balancer (ELB):** Distributes traffic across multiple backend servers to ensure availability.
* **API Gateway:** A single entry point for APIs. Handles authentication, rate limiting, and request transformation.

> [!TIP]
> **Interview Gold:** Always prefer **Horizontal Scaling** for production apps as it allows for high availability and redundancy.
