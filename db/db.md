---
layout: default
title: Database
nav_order: 7
---

# 🗄️ Database Interview Questions

A comprehensive guide to SQL and NoSQL databases, performance tuning, and architectural principles.

---

## 📑 Table of Contents
- [SQL Fundamentals](#-sql-fundamentals)
- [Performance: Indexing](#-performance-indexing)
- [ACID Properties](#-acid-properties)
- [Scaling: Replication & Sharding](#-scaling-replication--sharding)
- [NoSQL & MongoDB Optimization](#-nosql--mongodb-optimization)
- [Architecture: CAP Theorem](#-architecture-cap-theorem)
- [API Design: GraphQL vs REST](#-api-design-graphql-vs-rest)

---

## 🏗️ SQL Fundamentals

### `WHERE` vs `HAVING`
* **`WHERE`:** Filters rows **before** grouping (used on individual rows).
* **`HAVING`:** Filters results **after** `GROUP BY` (used on aggregated values).

### Joins
* **INNER JOIN:** Only matching records from both tables.
* **LEFT JOIN:** All records from the left table, plus matches from the right.
* **FULL JOIN:** All records from both tables, with NULLs where no match exists.

---

## ⚡ Performance: Indexing

An index is a data structure (usually a **B-Tree**) that improves the speed of data retrieval at the cost of slower writes.

### When to Avoid Indexes:
* Small tables.
* Columns with low cardinality (e.g., gender).
* Frequently updated columns.

> [!TIP]
> **Interview Gold:** Use `EXPLAIN ANALYZE` to see if your query is using an index or performing a slow full-table scan.

---

## 🔐 ACID Properties

1. **Atomicity:** All parts of a transaction succeed, or none do.
2. **Consistency:** Data must meet all validation rules after a transaction.
3. **Isolation:** Transactions do not interfere with each other.
4. **Durability:** Committed data is permanent, even after a crash.

---

## 📈 Scaling: Replication & Sharding

* **Read Replicas:** Copying the database to multiple servers to handle high read traffic.
* **Connection Pooling:** Reusing a set of active connections to reduce the overhead of opening new ones.
* **Sharding:** Splitting a large dataset across multiple database instances based on a shard key.

---

## 🍃 NoSQL & MongoDB Optimization

### Optimization Techniques:
* **Projection:** Return only the fields you need (`.find({}, {name: 1})`).
* **Pagination:** Use **Cursor-based pagination** (using `_id > lastId`) instead of Offset-based (`skip()`) for large datasets.
* **Lean Queries:** In Mongoose, use `.lean()` to return plain JS objects instead of heavy Mongoose documents.

---

## 🏗️ Architecture: CAP Theorem

A distributed system can only provide **two** of these three guarantees:
1. **Consistency:** Every node sees the same data at the same time.
2. **Availability:** Every request receives a response (even if it's old data).
3. **Partition Tolerance:** The system continues to work even if communication between nodes fails.

---

## 🌐 API Design: GraphQL vs REST

| Feature | REST | GraphQL |
| :--- | :--- | :--- |
| **Data Fetching** | Fixed endpoints (Over-fetching) | Client requests specific fields |
| **Requests** | Multiple calls for related data | Single request (Under-fetching fix) |
| **Typing** | Optional | Strongly typed schema |
| **Caching** | Built-in via HTTP | Complex to implement |

> [!IMPORTANT]
> **Over-fetching:** Getting more data than you need (REST).
> **Under-fetching:** Needing to make multiple calls to get all required data (REST).
