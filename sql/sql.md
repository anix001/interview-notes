---
layout: default
title: SQL
nav_order: 8
---

# 💾 SQL Interview Questions

Practical SQL queries, window functions, and optimization techniques for relational databases.

---

## 📑 Table of Contents
- [Essential Queries](#-essential-queries)
- [Window Functions](#-window-functions)
- [Joins & Set Operations](#-joins--set-operations)
- [Optimization & Stored Procedures](#-optimization--stored-procedures)
- [Data Maintenance](#-data-maintenance)

---

## 🏗️ Essential Queries

### 1️⃣ Find the Second Highest Salary
```sql
SELECT MAX(salary)
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```

### 2️⃣ Find Duplicate Records
```sql
SELECT email, COUNT(*)
FROM users
GROUP BY email
HAVING COUNT(*) > 1;
```

### 3️⃣ Employees Earning More than Their Manager
```sql
SELECT e.name
FROM employees e
JOIN employees m ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

---

## 🪟 Window Functions

Window functions perform calculations across a set of table rows that are related to the current row.

### `ROW_NUMBER()` vs `DENSE_RANK()`
* **`ROW_NUMBER()`:** Assigns a unique, sequential number to each row (1, 2, 3, 4).
* **`DENSE_RANK()`:** Assigns the same rank to duplicate values without skipping (1, 2, 2, 3).

### Example: Top 3 Earners per Department
```sql
WITH RankedEmployees AS (
    SELECT name, dept_id, salary,
    DENSE_RANK() OVER (PARTITION BY dept_id ORDER BY salary DESC) as rank
    FROM employees
)
SELECT * FROM RankedEmployees WHERE rank <= 3;
```

---

## 🔗 Joins & Set Operations

### `UNION` vs `UNION ALL`
* **`UNION`:** Combines result sets and **removes duplicates**.
* **`UNION ALL`:** Combines result sets **including duplicates**. (Faster as it skips uniqueness checks).

### `WHERE` vs `HAVING`
* **`WHERE`:** Filters rows before aggregation.
* **`HAVING`:** Filters groups after aggregation.

---

## ⚡ Optimization & Stored Procedures

### Query Optimization Checklist:
1. **Indexes:** Ensure columns used in `JOIN`, `WHERE`, and `ORDER BY` are indexed.
2. **Selectivity:** Avoid `SELECT *`; fetch only specific columns.
3. **Analyze:** Use `EXPLAIN` to identify bottlenecks.
4. **Prefer UNION ALL:** Use instead of `UNION` if you know there are no duplicates.

### Stored Procedures
Precompiled SQL statements stored in the DB.
* **Benefits:** Improved performance, code reusability, and enhanced security.

---

## 🧹 Data Maintenance

### Delete Duplicates (Keep One)
```sql
DELETE FROM users
WHERE id NOT IN (
   SELECT MIN(id)
   FROM users
   GROUP BY email
);
```

### Triggers
Automatic actions executed in response to `INSERT`, `UPDATE`, or `DELETE`.
* **Use Case:** Audit trails or data validation.

> [!TIP]
> **Interview Gold:** When asked to optimize a query, always mention **Indexing** and **Execution Plans** (`EXPLAIN`) first.
