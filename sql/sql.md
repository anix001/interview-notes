---
layout: default
title: SQL
nav_order: 8
---

---
layout: default
title: SQL
nav_order: 8
---

# 💾 SQL Interview Questions

Practical SQL queries, window functions, and optimization techniques for relational databases.

---

## 📑 Table of Contents
- [1️⃣ Find the Second Highest Salary](#1️⃣-find-the-second-highest-salary)
- [2️⃣ Joins Explained](#2️⃣-joins-explained)
- [3️⃣ UNION vs UNION ALL](#3️⃣-union-vs-union-all)
- [4️⃣ WHERE vs HAVING](#4️⃣-where-vs-having)
- [5️⃣ Find Duplicate Records](#5️⃣-find-duplicate-records)
- [6️⃣ Top 3 Earners per Department (CTE)](#6️⃣-top-3-earners-per-department-cte)
- [7️⃣ Handling NULL Values](#7️⃣-handling-null-values)
- [8️⃣ Stored Procedures](#8️⃣-stored-procedures)
- [9️⃣ Query Optimization](#9️⃣-query-optimization)
- [🔟 Database Triggers](#🔟-database-triggers)
- [1️⃣1️⃣ ROW_NUMBER() vs DENSE_RANK()](#1️⃣1️⃣-rownumber-vs-denserank)
- [1️⃣2️⃣ Delete Duplicates (Keep One)](#1️⃣2️⃣-delete-duplicates-keep-one)
- [1️⃣3️⃣ Employees Earning More than Their Manager](#1️⃣3️⃣-employees-earning-more-than-their-manager)
- [1️⃣4️⃣ Latest Record per Group](#1️⃣4️⃣-latest-record-per-group)

---

## **1️⃣ Find the Second Highest Salary**

### **Using a subquery:**
```sql
SELECT MAX(salary)
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```

### **Using `LIMIT` and `OFFSET`:**
```sql
SELECT email, COUNT(*)
FROM users
GROUP BY email
HAVING COUNT(*) > 1;
```

### **Using `DENSE_RANK()`:**
```sql
SELECT e.name
FROM employees e
JOIN employees m ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

---

## **2️⃣ Joins Explained**

* **INNER JOIN**: Returns only matching rows from both tables.
* **LEFT JOIN**: Returns all rows from the left table and matched rows from the right table. If no match, returns `NULL` for right-side columns.
* **FULL OUTER JOIN**: Returns all rows from both tables, filling with `NULL` where no match exists.

---

## **3️⃣ UNION vs UNION ALL**

* **UNION**: Combines two result sets and **removes duplicate rows**.
* **UNION ALL**: Combines two result sets including all duplicates. **UNION ALL** is generally faster because it doesn't need to perform a uniqueness check.

---

## **4️⃣ WHERE vs HAVING**

* **WHERE**: Filters rows **before** any grouping or aggregation (applied to individual rows).
* **HAVING**: Filters rows **after** `GROUP BY` and aggregation functions (applied to groups).

---

## **5️⃣ Find Duplicate Records**

```sql
SELECT email, COUNT(*)
FROM users
GROUP BY email
HAVING COUNT(*) > 1;
```

---

## **6️⃣ Top 3 Earners per Department (CTE)**

```sql
WITH RankedEmployees AS (
    SELECT name, dept_id, salary,
    ROW_NUMBER() OVER (PARTITION BY dept_id ORDER BY salary DESC) AS rn
    FROM employees
)
SELECT * FROM RankedEmployees WHERE rn <= 3;
```

---

## **7️⃣ Handling NULL Values**

Use `IS NULL`, `IS NOT NULL`, or `COALESCE()` to replace NULLs:
```sql
SELECT COALESCE(salary, 0) FROM employees;
```

---

## **8️⃣ Stored Procedures**

A **stored procedure** is a precompiled set of SQL statements stored in the database.
* **Reusability**: Write once, execute many times.
* **Performance**: Precompiled by the database, leading to faster execution.
* **Security**: Users can execute procedures without direct access to underlying tables.
* **Reduced Traffic**: Sends a single procedure call over the network instead of multiple queries.

---

## **9️⃣ Query Optimization**

1. **Indexes**: Add indexes on frequently filtered or joined columns.
2. **Selective Fetching**: Avoid `SELECT *`; fetch only required columns.
3. **Analyze Execution**: Use `EXPLAIN` to analyze query execution plans.
4. **Avoid UNION**: Prefer `UNION ALL` if duplicates are not an issue.

> [!TIP]
> **Interview Gold:** When asked about optimization, always mention **Indexing** and **Execution Plans** (`EXPLAIN`) as your first steps.

---

## **🔟 Database Triggers**

A **trigger** is a database object that automatically executes SQL statements when an event (INSERT, UPDATE, or DELETE) occurs.
* **BEFORE Trigger**: Runs before the event (useful for validation).
* **AFTER Trigger**: Runs after the event (useful for logging/audit).

---

## **1️⃣1️⃣ ROW_NUMBER() vs DENSE_RANK()**

* **`ROW_NUMBER()`**: Assigns a unique, sequential number to each row (1, 2, 3, 4).
* **`DENSE_RANK()`**: Assigns the same rank to duplicate values without skipping the next rank (1, 2, 2, 3).

---

## **1️⃣2️⃣ Delete Duplicates (Keep One)**

```sql
DELETE FROM users
WHERE id NOT IN (
   SELECT MIN(id)
   FROM users
   GROUP BY email
);
```

---

## **1️⃣3️⃣ Employees Earning More than Their Manager**

```sql
SELECT e.name
FROM employees e
JOIN employees m ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

---

## **1️⃣4️⃣ Latest Record per Group**

```sql
SELECT *
FROM (
   SELECT *,
          ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY created_at DESC) rn
   FROM orders
) t
WHERE rn = 1;
```
