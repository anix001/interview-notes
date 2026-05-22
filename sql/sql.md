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

```sql
-- Sample tables:
-- employees: (id, name, dept_id)
-- departments: (id, dept_name)

-- INNER JOIN — only employees who belong to a known department
SELECT e.name, d.dept_name
FROM employees e
INNER JOIN departments d ON e.dept_id = d.id;

-- LEFT JOIN — all employees, even those without a department
SELECT e.name, d.dept_name
FROM employees e
LEFT JOIN departments d ON e.dept_id = d.id;
-- employees with no dept get NULL for dept_name

-- FULL OUTER JOIN — all employees + all departments
SELECT e.name, d.dept_name
FROM employees e
FULL OUTER JOIN departments d ON e.dept_id = d.id;
-- unmatched rows on both sides appear with NULLs
```

> [!TIP]
> **Interview Gold:** The most common interview trick is "find employees with no manager" or "find users who never placed an order" — always a `LEFT JOIN ... WHERE right_table.id IS NULL` pattern.

---

## **3️⃣ UNION vs UNION ALL**

* **UNION**: Combines two result sets and **removes duplicate rows**.
* **UNION ALL**: Combines two result sets including all duplicates. **UNION ALL** is generally faster because it doesn't need to perform a uniqueness check.

```sql
-- Get all cities from customers and suppliers
SELECT city FROM customers
UNION                         -- removes duplicate cities
SELECT city FROM suppliers;
-- Result: Delhi, Mumbai, Pune  (no repeats even if both tables have "Delhi")

SELECT city FROM customers
UNION ALL                     -- keeps ALL rows including duplicates
SELECT city FROM suppliers;
-- Result: Delhi, Mumbai, Delhi, Pune  (Delhi appears twice)
```

> [!TIP]
> **Interview Gold:** Use `UNION ALL` by default — it's faster. Only switch to `UNION` when you actually need to eliminate duplicates. Adding `DISTINCT` to your query after a `UNION ALL` is sometimes cleaner than using `UNION`.

---

## **4️⃣ WHERE vs HAVING**

* **WHERE**: Filters rows **before** any grouping or aggregation (applied to individual rows).
* **HAVING**: Filters rows **after** `GROUP BY` and aggregation functions (applied to groups).

```sql
-- Count orders per customer, but only for customers in Delhi
-- AND only show customers with more than 3 orders

SELECT customer_id, COUNT(*) AS order_count
FROM orders
WHERE city = 'Delhi'                -- ← row-level filter (before grouping)
GROUP BY customer_id
HAVING COUNT(*) > 3;               -- ← group-level filter (after aggregation)
```

**Execution order:** `WHERE` → `GROUP BY` → `HAVING` → `SELECT` → `ORDER BY`

---

## **5️⃣ Find Duplicate Records**

```sql
SELECT email, COUNT(*)
FROM users
GROUP BY email
HAVING COUNT(*) > 1;
```

**Why can't you use `WHERE COUNT(*) > 1`?**
Because `WHERE` runs *before* `GROUP BY`. At the `WHERE` stage, `COUNT()` hasn't been calculated yet — the database doesn't know the count for each group. Only after `GROUP BY` groups the rows can `HAVING` filter on the count.

```
Execution order:
1. FROM users
2. GROUP BY email    ← rows are grouped here
3. COUNT(*) computed ← now we know the count per group
4. HAVING COUNT(*) > 1 ← filter groups (this is the only place you can use COUNT)
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

```sql
-- Create a stored procedure to get orders for a customer
DELIMITER $$
CREATE PROCEDURE GetCustomerOrders(IN customer_id INT)
BEGIN
  SELECT order_id, total, created_at
  FROM orders
  WHERE orders.customer_id = customer_id
  ORDER BY created_at DESC;
END $$
DELIMITER ;

-- Call it
CALL GetCustomerOrders(42);
```

**Simple Explanation:** Instead of sending a long SQL query from your app every time, you store the logic in the database once and just call it by name. Like calling a function — `CALL GetCustomerOrders(42)` instead of sending the full SQL string every request.

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

**Example — audit log trigger (fires after every salary update):**
```sql
-- Audit table to track changes
CREATE TABLE salary_audit (
  id INT AUTO_INCREMENT PRIMARY KEY,
  employee_id INT,
  old_salary DECIMAL(10,2),
  new_salary DECIMAL(10,2),
  changed_at TIMESTAMP DEFAULT NOW()
);

-- AFTER UPDATE trigger on employees table
CREATE TRIGGER log_salary_change
AFTER UPDATE ON employees
FOR EACH ROW
BEGIN
  IF OLD.salary <> NEW.salary THEN
    INSERT INTO salary_audit (employee_id, old_salary, new_salary)
    VALUES (NEW.id, OLD.salary, NEW.salary);
  END IF;
END;

-- Now every salary update is automatically logged:
UPDATE employees SET salary = 90000 WHERE id = 5;
-- salary_audit gets a new row automatically
```

> [!TIP]
> **Interview Gold:** Triggers are powerful but use them sparingly — they run invisibly, making debugging hard. Use them for audit logging and data validation where you want the rule enforced at the database level regardless of which app writes the data.

---

## **1️⃣1️⃣ ROW_NUMBER() vs DENSE_RANK()**

* **`ROW_NUMBER()`**: Assigns a unique, sequential number to each row (1, 2, 3, 4).
* **`DENSE_RANK()`**: Assigns the same rank to duplicate values without skipping the next rank (1, 2, 2, 3).

**Side-by-side output — same data, different results:**

| name  | salary | ROW_NUMBER() | RANK() | DENSE_RANK() |
|-------|--------|-------------|--------|--------------|
| Alice | 90000  | 1           | 1      | 1            |
| Bob   | 80000  | 2           | 2      | 2            |
| Carol | 80000  | 3           | 2      | 2            |
| Dave  | 70000  | 4           | 4      | 3            |

* `ROW_NUMBER` — always unique, no ties (1,2,3,4)
* `RANK` — ties get the same rank, **skips** the next number (1,2,2,**4**)
* `DENSE_RANK` — ties get the same rank, **no skip** (1,2,2,**3**)

```sql
SELECT name, salary,
  ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num,
  RANK()       OVER (ORDER BY salary DESC) AS rnk,
  DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rnk
FROM employees;
```

> [!TIP]
> **Interview Gold:** Use `DENSE_RANK` for leaderboards/rankings where tied entries share a rank and the next position continues without gaps. Use `ROW_NUMBER` when you need a unique ID per row regardless of ties (e.g., pagination, deleting duplicates).

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

---

## **1️⃣5️⃣ LAG() and LEAD() Window Functions**

`LAG()` gets the value from the **previous row**. `LEAD()` gets the value from the **next row**. Both are window functions — they don't collapse rows like `GROUP BY`.

```sql
SELECT
  employee_id,
  salary,
  LAG(salary)  OVER (ORDER BY hire_date) AS prev_salary,
  LEAD(salary) OVER (ORDER BY hire_date) AS next_salary
FROM employees;
```

**Use case:** Compare a row to its neighbours — e.g., month-over-month revenue growth, previous order status, consecutive log entries.

```sql
-- Month-over-month revenue change
SELECT
  month,
  revenue,
  revenue - LAG(revenue) OVER (ORDER BY month) AS change
FROM monthly_sales;
```

> [!TIP]
> **Interview Gold:** `LAG`/`LEAD` are the SQL way to do "compare this row to the previous one" without a self-join. Much cleaner and faster than `JOIN employees e2 ON e2.hire_date = ...`.

---

## **1️⃣6️⃣ Subquery vs JOIN — Which is Faster?**

Both achieve similar results but work differently.

### **Subquery**
Runs a nested query inside the main query. Simple to read but can be slow if the subquery runs once per row (correlated subquery).

```sql
-- Find employees who earn more than the average salary
SELECT name FROM employees
WHERE salary > (SELECT AVG(salary) FROM employees);
```

### **JOIN**
Combines tables. Usually faster because the database can optimize the join using indexes.

```sql
-- Same result using JOIN
SELECT e.name FROM employees e
JOIN (SELECT AVG(salary) avg_sal FROM employees) avg ON e.salary > avg.avg_sal;
```

### **Rule of thumb**
* **Non-correlated subquery** (runs once): similar performance to JOIN.
* **Correlated subquery** (runs per row): almost always slower — rewrite as a JOIN or use a CTE.

> [!TIP]
> **Interview Gold:** If your query is slow and you have a subquery in the `WHERE` clause, check if it's correlated. Converting it to a `JOIN` or `CTE` is usually the fix.

---

## **1️⃣7️⃣ Transaction Isolation Levels**

Isolation levels control how much one transaction can "see" from other concurrent transactions.

| Level | Dirty Read | Non-Repeatable Read | Phantom Read |
|-------|-----------|---------------------|--------------|
| **READ UNCOMMITTED** | ✅ possible | ✅ possible | ✅ possible |
| **READ COMMITTED** | ❌ blocked | ✅ possible | ✅ possible |
| **REPEATABLE READ** | ❌ blocked | ❌ blocked | ✅ possible |
| **SERIALIZABLE** | ❌ blocked | ❌ blocked | ❌ blocked |

### **Simple definitions**
* **Dirty Read:** You read data another transaction hasn't committed yet (might be rolled back).
* **Non-Repeatable Read:** You read the same row twice in a transaction and get different values because another transaction updated it in between.
* **Phantom Read:** You re-run a query and get new rows because another transaction inserted data.

### **In practice**
* Most databases default to **READ COMMITTED** (PostgreSQL) or **REPEATABLE READ** (MySQL InnoDB).
* Use **SERIALIZABLE** only when data correctness is critical (financial transfers) — it's the slowest.

> [!TIP]
> **Interview Gold:** The default for most apps is **READ COMMITTED**. If you're doing a multi-step bank transfer, bump it to **SERIALIZABLE** for that transaction only so no phantom rows sneak in between your balance check and debit.
