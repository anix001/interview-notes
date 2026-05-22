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

**Example — both in the same query:**
```sql
-- Find departments with more than 5 employees, but only count active ones
SELECT department, COUNT(*) AS total
FROM employees
WHERE status = 'active'          -- ← WHERE filters BEFORE grouping (row level)
GROUP BY department
HAVING COUNT(*) > 5;             -- ← HAVING filters AFTER grouping (group level)
```

> [!TIP]
> **Interview Gold:** You can't use `HAVING COUNT(*) > 5` as `WHERE COUNT(*) > 5` because `WHERE` runs before `COUNT()` is calculated. `HAVING` is the only way to filter on aggregated values.

### **Joins Explained**
* **INNER JOIN**: Only matching records from both tables.
* **LEFT JOIN**: All records from the left table + matched records from the right.
* **RIGHT JOIN**: All records from the right table + matched records from the left.

**Think of it with two lists: Students and Grades:**
```sql
-- INNER JOIN — only students who HAVE a grade
SELECT s.name, g.score
FROM students s
INNER JOIN grades g ON s.id = g.student_id;
-- Result: Alice(90), Bob(75)  — Charlie (no grade) is excluded

-- LEFT JOIN — all students, grade or not
SELECT s.name, g.score
FROM students s
LEFT JOIN grades g ON s.id = g.student_id;
-- Result: Alice(90), Bob(75), Charlie(NULL)  — Charlie included with NULL

-- FULL OUTER JOIN — everyone from both sides
SELECT s.name, g.score
FROM students s
FULL OUTER JOIN grades g ON s.id = g.student_id;
-- Result: everyone, with NULLs where no match exists on either side
```

> [!TIP]
> **Interview Gold:** `LEFT JOIN` is the most commonly used join in real apps — "give me all users and their orders, even users with no orders." `INNER JOIN` is second. `RIGHT JOIN` is rare (just flip the tables and use `LEFT JOIN`).

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

**Simple Explanation:** ACID is what makes a bank transfer safe. Without it: you deduct $100 from Account A, then the server crashes before adding $100 to Account B. The $100 vanishes. ACID's **Atomicity** rule says "all or nothing" — if step 2 fails, step 1 is automatically rolled back.

```sql
BEGIN; -- start the transaction

UPDATE accounts SET balance = balance - 100 WHERE id = 1; -- deduct from Alice
UPDATE accounts SET balance = balance + 100 WHERE id = 2; -- add to Bob

-- If both succeed:
COMMIT; -- make it permanent

-- If anything fails (server crash, insufficient balance):
ROLLBACK; -- undo ALL changes — Alice keeps her $100
```

**What breaks without each property:**
* Without **Atomicity** → partial updates (money disappears)
* Without **Consistency** → balance goes negative (invalid state)
* Without **Isolation** → two people book the last seat on a flight simultaneously
* Without **Durability** → committed data lost on power failure

> [!TIP]
> **Interview Gold:** The classic ACID example is always a bank transfer. Know it cold. "Atomicity means all-or-nothing. If the debit succeeds but the credit fails, the whole transaction rolls back."

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

**Side-by-side schema example:**

```sql
-- ❌ Unnormalized (redundant data)
orders table:
| order_id | customer_name | customer_email   | product_name | price |
|----------|---------------|------------------|--------------|-------|
| 1        | Alice         | alice@email.com   | Laptop       | 999   |
| 2        | Alice         | alice@email.com   | Mouse        | 29    |
-- Problem: if Alice changes her email, you update 2 rows (or more!)

-- ✅ Normalized (3NF — each fact stored once)
customers table: | id | name  | email           |
orders table:    | id | customer_id | product_id |
products table:  | id | name   | price          |
-- Alice's email is stored once. One update fixes everything.

-- ✅ Denormalized (for read-heavy reporting)
order_summary table:
| order_id | customer_name | product_name | price |
-- Redundant, but avoids 3-way JOIN on every report query
```

> [!TIP]
> **Interview Gold:** Normalize for **writes** (no redundancy = no update anomalies). Denormalize for **reads** (fewer joins = faster queries). Most real apps do both — normalized core tables + denormalized reporting/analytics tables or caches.

---

## **6️⃣ MongoDB & NoSQL Optimization**

### **Strategies for faster APIs:**
1. **Project only needed fields:** `db.users.find({}, { name: 1, email: 1 })`.
2. **Use lean queries (Mongoose):** Skips Mongoose document overhead.
3. **Aggregation Pipeline:** Perform filtering and grouping in the DB (`$match`, `$group`, `$lookup`).
4. **Embedded vs Referenced:** 
    * **Embedded:** Faster reads, good for "one-to-few".
    * **Referenced:** More scalable, avoids duplication.

**Code comparison:**
```javascript
// ✅ Embedded — user's address is stored inside the user document
// Good: one DB read gets everything. Bad: if address format changes, update many docs.
{
  _id: "u1",
  name: "Alice",
  address: { street: "123 Main St", city: "Delhi", zip: "110001" }
}

// ✅ Referenced — address is a separate document, linked by ID
// Good: address can be updated independently. Bad: needs a second query or $lookup.
// User doc:
{ _id: "u1", name: "Alice", addressId: "a1" }

// Address doc:
{ _id: "a1", street: "123 Main St", city: "Delhi", zip: "110001" }
```

**Rule of thumb:**
* Embed when: data is always accessed together and the embedded array is small (< 100 items)
* Reference when: the sub-document is large, frequently updated, or shared across documents

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

---

## **1️⃣1️⃣ ORM (Prisma & Mongoose)**

An **ORM (Object-Relational Mapper)** lets you interact with a database using your programming language instead of writing raw SQL or query strings.

### **Prisma (SQL databases)**
* Define your schema in `schema.prisma`. Prisma generates a fully type-safe client.
* Auto-generates migrations.
```javascript
// Instead of: SELECT * FROM users WHERE id = 1
const user = await prisma.user.findUnique({ where: { id: 1 } });
```

### **Mongoose (MongoDB)**
* Adds schemas and validation on top of MongoDB's flexible documents.
```javascript
const UserSchema = new Schema({ name: String, email: String });
const User = model("User", UserSchema);
const user = await User.findById("abc123");
```

### **ORM pros & cons**
* ✅ Less boilerplate, type safety, easy migrations.
* ❌ Can generate inefficient queries for complex joins. Always check the generated SQL in production.

> [!TIP]
> **Interview Gold:** ORMs are great for productivity but can hide performance problems. For complex reporting queries, write raw SQL. For CRUD operations, use the ORM.

---

## **1️⃣2️⃣ Optimistic vs Pessimistic Locking**

Both prevent data corruption when multiple users try to update the same record at the same time.

### **Pessimistic Locking**
* Lock the row when you read it. No one else can update it until you're done.
* Safe but slow — other users are blocked.
* Use for: bank transfers, inventory deductions (when correctness is critical).
```sql
SELECT * FROM accounts WHERE id = 1 FOR UPDATE; -- locks the row
```

### **Optimistic Locking**
* Don't lock anything. When you save, check if the record was changed by someone else since you read it (using a version number).
* Fast but requires retry logic on conflict.
* Use for: document editing, low-conflict scenarios.
```sql
UPDATE posts SET title = 'New', version = 2
WHERE id = 1 AND version = 1; -- fails if version changed
```

> [!TIP]
> **Interview Gold:** Optimistic locking is preferred for most web apps because conflicts are rare. Pessimistic locking is for financial systems where you can't afford to retry on conflict.

---

## **1️⃣3️⃣ Soft Delete Pattern**

Instead of permanently deleting a row, mark it as deleted with a flag. The row stays in the database.

```sql
ALTER TABLE users ADD COLUMN deleted_at TIMESTAMP NULL;

-- Soft delete
UPDATE users SET deleted_at = NOW() WHERE id = 123;

-- Query active users only
SELECT * FROM users WHERE deleted_at IS NULL;
```

### **Why use it?**
* **Audit trail:** You can see what was deleted and when.
* **Recovery:** Easily restore a deleted record.
* **Referential integrity:** Foreign keys still work (no orphaned records).

### **Drawbacks**
* All queries need a `WHERE deleted_at IS NULL` filter — easy to forget.
* Table grows forever. Archive old deleted rows periodically.

> [!TIP]
> **Interview Gold:** Always ask in the interview: "Should deletes be soft or hard?" For anything involving user data, billing records, or audit requirements — always soft delete.
