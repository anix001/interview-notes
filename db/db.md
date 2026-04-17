---
layout: default
title: Database
nav_order: 6
---

# 🗄️ Database Interview Questions

Relational (SQL) and Non-Relational (NoSQL/MongoDB) database concepts, optimization, and architecture.

---

## 📑 Table of Contents
- [1️⃣ SQL Fundamentals](#1️⃣-sql-fundamentals)
- [2️⃣ Indexing & Performance](#2️⃣-indexing--performance)
- [3️⃣ Transactions & ACID](#3️⃣-transactions--acid)
- [4️⃣ Scaling & High Availability](#4️⃣-scaling--high-availability)
- [5️⃣ Normalization vs Denormalization](#5️⃣-normalization--vs-denormalization)
- [6️⃣ MongoDB & NoSQL Optimization](#6️⃣-mongodb--nosql-optimization)
- [7️⃣ Pagination Strategies](#7️⃣-pagination-strategies)
- [8️⃣ CAP Theorem](#8️⃣-cap-theorem)
- [9️⃣ SQL vs NoSQL](#9️⃣-sql-vs-nosql)
- [🔟 GraphQL vs REST](#🔟-graphql-vs-rest)

---

## **1️⃣ SQL Fundamentals**

### **WHERE vs HAVING**
* **`WHERE`**: Filters rows **before** any grouping or aggregation.
* **`HAVING`**: Filters results **after** `GROUP BY` and aggregation functions.

### **Joins Explained**
* **INNER JOIN**: Only matching records from both tables.
* **LEFT JOIN**: All records from the left table + matched records from the right.
* **RIGHT JOIN**: All records from the right table + matched records from the left.

---

## **2️⃣ Indexing & Performance**

An **index** is a data structure (usually a B-Tree) that improves search speed by avoiding a full table scan.

### **When NOT to use an index:**
* On very small tables.
* On columns with low cardinality (e.g., `gender`, `isActive`).
* On frequently updated columns (indexes slow down `INSERT`/`UPDATE`).

> [!TIP]
> **Interview Gold:** If a query is slow despite an index, check if you're using `LIKE '%abc'` (which breaks index usage), applying functions on the column (e.g., `WHERE LOWER(name)`), or if the column order in a compound index is incorrect. Use **`EXPLAIN`** to analyze.

---

## **3️⃣ Transactions & ACID**

* **Atomicity**: All operations in a transaction succeed, or none do.
* **Consistency**: The database moves from one valid state to another.
* **Isolation**: Transactions don't interfere with each other.
* **Durability**: Once committed, data survives system crashes.

---

## **4️⃣ Scaling & High Availability**

### **How to handle 1 million+ users:**
1. **Indexing:** Efficient lookups.
2. **Read Replicas:** Offload read traffic.
3. **Caching:** Use Redis to store frequent queries.
4. **Connection Pooling:** Reuse database connections.
5. **Sharding:** Partition data across multiple servers.

---

## **5️⃣ Normalization vs Denormalization**

* **Normalization:** Organizing data into multiple tables to reduce redundancy and improve consistency (e.g., 1NF, 2NF, 3NF).
* **Denormalization:** Intentionally adding redundancy to improve **read performance** by reducing joins.

---

## **6️⃣ MongoDB & NoSQL Optimization**

### **Strategies for faster APIs:**
1. **Project only needed fields:** `db.users.find({}, { name: 1, email: 1 })`.
2. **Use lean queries (Mongoose):** Skips Mongoose document overhead.
3. **Aggregation Pipeline:** Perform filtering and grouping in the DB (`$match`, `$group`, `$lookup`).
4. **Embedded vs Referenced:** 
    * **Embedded:** Faster reads, good for "one-to-few".
    * **Referenced:** More scalable, avoids duplication.

---

## **7️⃣ Pagination Strategies**

### **Offset Pagination (`skip` + `limit`)**
* **Cons:** Becomes very slow on large datasets as the DB must scan and skip N documents.

### **Cursor-Based Pagination**
* **How:** Use a unique, indexed key (like `_id` or `createdAt`) as a pointer.
* **Pros:** Scalable and avoids missing data if new records are added between pages.

---

## **8️⃣ CAP Theorem**

A distributed system can only guarantee 2 out of 3:
1. **Consistency (C):** Every node sees the same data at the same time.
2. **Availability (A):** Every request receives a response (success/failure).
3. **Partition Tolerance (P):** System continues to operate despite network failures.

> [!NOTE]
> Most modern distributed databases prioritize **P** and choose between **C** or **A**.

---

## **9️⃣ SQL vs NoSQL**

* **SQL:** Fixed schema, ACID compliant, complex joins. Best for banking, ERP, and structured data.
* **NoSQL:** Flexible schema, horizontal scaling, high-speed. Best for social media, real-time feeds, and big data.

---

## **🔟 GraphQL vs REST**

* **REST:** Fixed endpoints. Can lead to **over-fetching** (getting more data than needed) or **under-fetching** (needing multiple calls).
* **GraphQL:** Client requests exactly what it needs in a single call.
* **Winner:** GraphQL is better for complex frontend needs; REST is better for simple CRUD and standard HTTP caching.
