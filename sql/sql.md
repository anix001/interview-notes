# 💾 SQL Interview Questions

## **1️⃣ Write a query to find the second highest salary?**

**Answer:**

Using a subquery:
```sql
SELECT MAX(salary)
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);
```

Using `LIMIT` and `OFFSET`:
```sql
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
LIMIT 1 OFFSET 1;
```

Using `DENSE_RANK()`:
```sql
SELECT salary
FROM (
   SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
   FROM employees
) t
WHERE rnk = 2;
```

## **2️⃣ Explain INNER JOIN, LEFT JOIN, and FULL OUTER JOIN?**

**Answer:**

* **INNER JOIN**: Returns only matching rows from both tables.
* **LEFT JOIN**: Returns all rows from the left table and matched rows from the right table. If no match, returns `NULL` for right-side columns.
* **FULL OUTER JOIN**: Returns all rows from both tables, filling with `NULL` where no match exists.

## **3️⃣ What is the difference between UNION and UNION ALL?**

**Answer:**

* **UNION**: Combines two result sets and removes duplicate rows.
* **UNION ALL**: Combines two result sets including all duplicates. **UNION ALL** is generally faster because it doesn't need to perform a uniqueness check.

## **4️⃣ What is the difference between WHERE and HAVING?**

**Answer:**

* **WHERE**: Filters rows **before** any grouping or aggregation (applied to individual rows).
* **HAVING**: Filters rows **after** `GROUP BY` and aggregation functions (applied to groups).

## **5️⃣ How do you find duplicate records in a table?**

**Answer:**

Use `GROUP BY` with `HAVING COUNT(*) > 1`:
```sql
SELECT column_name, COUNT(*)
FROM table_name
GROUP BY column_name
HAVING COUNT(*) > 1;
```

## **6️⃣ Use a CTE to find the top 3 earners in each department?**

**Answer:**

```sql
WITH RankedEmployees AS (
    SELECT employee_id, department_id, salary,
    ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary DESC) AS rn
    FROM employees
)
SELECT employee_id, department_id, salary
FROM RankedEmployees
WHERE rn <= 3;
```

## **7️⃣ How do you handle NULL values in SQL?**

**Answer:**

Use `IS NULL`, `IS NOT NULL`, or `COALESCE()` to replace NULLs:
```sql
SELECT COALESCE(salary, 0) FROM employees;
```

## **8️⃣ Why do we use Stored Procedures?**

**Answer:**

A **stored procedure** is a precompiled set of SQL statements stored in the database.

* **Reusability**: Write once, execute many times.
* **Performance**: Precompiled by the database, leading to faster execution.
* **Security**: Users can execute procedures without direct access to underlying tables.
* **Reduced Traffic**: Sends a single procedure call over the network instead of multiple queries.

## **9️⃣ How do you optimize a slow SQL query?**

**Answer:**

1. **Indexes**: Add indexes on frequently filtered or joined columns.
2. **Selective Fetching**: Avoid `SELECT *`; fetch only required columns.
3. **Analyze Execution**: Use `EXPLAIN` to analyze query execution plans.
4. **Avoid UNION**: Prefer `UNION ALL` if duplicates are not an issue.

## **🔟 What is a Database Trigger?**

**Answer:**

A **trigger** is a database object that automatically executes a set of SQL statements when an event (INSERT, UPDATE, or DELETE) occurs on a table.

* **BEFORE Trigger**: Runs before the event happens (useful for data validation).
* **AFTER Trigger**: Runs after the event happens (useful for logging or audit trails).

## **1️⃣1️⃣ What is the difference between ROW_NUMBER() and DENSE_RANK()?**

**Answer:**

* **ROW_NUMBER()**: Assigns a unique, sequential number to each row (e.g., 1, 2, 3, 4).
* **DENSE_RANK()**: Assigns the same rank to duplicate values, but doesn't skip the next rank (e.g., 1, 2, 2, 3).

## **1️⃣2️⃣ How do you delete duplicate records but keep one?**

**Answer:**

Using a subquery:
```sql
DELETE FROM users
WHERE id NOT IN (
   SELECT MIN(id)
   FROM users
   GROUP BY email
);
```

## **1️⃣3️⃣ How do you find employees earning more than their manager?**

**Answer:**

```sql
SELECT e.name
FROM employees e
JOIN employees m
ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

## **1️⃣4️⃣ How do you find the latest record per group?**

**Answer:**

Using `ROW_NUMBER()`:
```sql
SELECT *
FROM (
   SELECT *,
          ROW_NUMBER() OVER (PARTITION BY user_id ORDER BY created_at DESC) rn
   FROM orders
) t
WHERE rn = 1;
```
