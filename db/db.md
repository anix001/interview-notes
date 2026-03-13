### **DB RELATED QUESTIONS:**

# **🧱 1\. Basic SQL (Must Be Strong)**

## **✅ Q1: What is the difference between `WHERE` and `HAVING`?**

### **🎯 Interviewer checks: Do you understand aggregation?**

### **💡 Answer:**

* ### **`WHERE` filters rows before grouping** 

* ### **`HAVING` filters results after GROUP BY** 

## **✅ Q2: Difference between `INNER JOIN`, `LEFT JOIN`, `RIGHT JOIN`**

🎯 Checks: Join understanding

💡 Answer:

* `INNER JOIN` → only matching records

* `LEFT JOIN` → all left \+ matched right

* `RIGHT JOIN` → all right \+ matched left

# **⚡ 2\. Indexing (Very Important for Backend Dev)**

## **✅ Q3: What is an index?**

🎯 Checks: Performance knowledge

💡 Answer:

An index is a data structure (usually B-Tree) that improves search speed.

Without index → Full table scan  
 With index → Logarithmic search

## **✅ Q4: When should you NOT use an index?**

🎯 Checks: Real understanding

💡 Good Answer:

* On very small tables

* On columns with low cardinality (like gender)

* On frequently updated columns

* When writes are very frequent (index slows inserts/updates)

## **✅ Q5: Why is my query slow even though I added an index?**

🎯 Strong question.

💡 Possible reasons:

* Index not being used (wrong column order)

* Using `LIKE '%abc'`

* Function on indexed column: `WHERE LOWER(name)`

* Too many OR conditions

* Outdated statistics

🧠 Advanced: Use `EXPLAIN` to analyze.

# **🔐 3\. Transactions & ACID**

## **✅ Q6: What is ACID?**

💡 Answer:

* Atomicity → All or nothing

* Consistency → Valid state only

* Isolation → No dirty interference

* Durability → Data survives crash

## **✅ Q9: How do you handle 1 million users?**

🎯 Interviewers love this.

Answer:

* Add indexes

* Read replicas

* Caching (Redis)

* Connection pooling

* Horizontal scaling

* Sharding if needed

# **🔥 5\. Real Backend Developer Questions**

## **✅ Q10: How do you prevent duplicate records?**

Answer:

* Unique constraints: 
```bash
db.users.createIndex({ email: 1 }, { unique: true })
```

* Transactions: Used when multiple operations must succeed together.

* Upsert:  (Update if exists, Insert if not)

* Proper locking:Used when multiple requests happen at same time.

## **✅ Q11: Difference between Normalization & Denormalization?**

Normalization:

* Reduce redundancy

* Better consistency

Denormalization:

* Improve read performance

* Add redundancy intentionally

## **✅ Q12: Difference between embedded vs referenced documents?**

Embedded:

* Faster reads

* Good for one-to-few

Referenced:

* Scalable

* Avoid duplication

## **✅ Q13: What is an aggregation pipeline?**

Stages:

* $match

* $group

* $lookup

* $project

## **Normalization:** 

Normalization means **organizing database tables to remove duplicate data and avoid problems when inserting, updating, or deleting.**

| studentId | studentName | course | instructor | instructorPhone |
| ----- | ----- | ----- | ----- | ----- |
| 1 | Anish | Math | Ram | 9800000001 |
| 1 | Anish | Science | Hari | 9800000002 |
| 2 | Sita | Math | Ram | 9800000001 |
| 3 | John | Science | Hari | 9800000002 |

# **❌ Problems here (This is why normalization exists)**

### **Problem 1: Duplicate data**

Ram and his phone appear multiple times.

### **Problem 2: Update problem**

If Ram's phone changes, you must update multiple rows.

If you miss one → inconsistent data.

---

### **Problem 3: Insert problem**

You cannot add instructor unless student exists.

---

### **Problem 4: Delete problem**

If you delete all Math students → Ram info lost.

# **✅ Solution: Normalize into separate tables**

---

# **Step 1: Create Students table**

| studentId | studentName |
| ----- | ----- |
| 1 | Anish |
| 2 | Sita |
| 3 | John |

---

# **Step 2: Create Instructors table**

| instructorId | instructorName | instructorPhone |
| ----- | ----- | ----- |
| 1 | Ram | 9800000001 |
| 2 | Hari | 9800000002 |

---

# **Step 3: Create Courses table**

| courseId | courseName | instructorId |
| ----- | ----- | ----- |
| 1 | Math | 1 |
| 2 | Science | 2 |

---

# **Step 4: Create StudentCourses table**

| studentId | courseId |
| ----- | ----- |
| 1 | 1 |
| 1 | 2 |
| 2 | 1 |
| 3 | 2 |

---

# **✅ Now problems are solved**

No duplicate instructor phone  
 Easy updates  
 Easy inserts  
 Safe deletes

Normalization is the process of organizing database into multiple related tables to remove duplicate data and improve consistency.

# **🎯 Final Interview Answer (Best Answer)**

Say this confidently:

Normalization is used to remove duplicate data and organize database into multiple related tables. It improves data consistency and prevents update, insert, and delete problems. For example, instead of storing student, course, and instructor in one table, we create separate tables and link them using IDs.

# **Connection pooling**

Connection pooling means **reusing database connections instead of creating a new connection every time**.

This improves **performance and scalability**.

# **🎯 Simple real-world analogy**

Think of database connection like a **phone call** ☎️

❌ Without pooling:

* Every time you need something → dial number → talk → hang up

* This wastes time

✅ With pooling:

* You keep some calls already active

* Just reuse one when needed

  # **🎯 Why connection pooling is important (Interview answer)**

You can say this:

Connection pooling is a technique where a set of database connections is created and reused instead of opening a new connection for every request. It improves performance, reduces overhead, and prevents database overload.

## **JWKS (JSON Web Key Set)**

**JWKS (JSON Web Key Set)** is a **public key collection used to verify JWT tokens.**

**Simple interview answer:**

JWKS is a set of public keys provided by the authentication server that applications use to verify if a JWT token is valid and trusted.

---

**Even simpler:**

JWKS contains public keys used to verify JWT signatures.

---

**Example flow:**

1. User logs in → gets JWT

2. Your backend fetches JWKS from auth server

3. Uses public key from JWKS → verifies JWT

## **Stateless vs Stateful** 

* **Stateless** → Sending a **letter** each time. You include your ID in every letter. Server doesn’t remember you.

* **Stateful** → **Phone call** with operator remembering you from last call. Server keeps track of your session.

## **Optimizing MongoDB APIs**

#  **1\. Use Indexes Wisely**

* **What:** Index fields that are frequently queried or sorted.

* **Why:** Avoid full collection scans.

```bash
db.users.createIndex({ email: 1 });
```

**Tip:** Use **compound indexes** if querying multiple fields together.

# **2\. Project Only Needed Fields**

* **What:** Return only required fields instead of full documents.

* **Why:** Reduces network transfer & memory usage.

```bash
db.users.find({}, { name: 1, email: 1 });
```

# **3\. Limit & Paginate**

* **What:** Don’t return huge datasets at once.

* **Why:** Prevents server overload.

# **4\. Avoid $where & Regex Without Index**

* **What:** Don’t use `$where` or unanchored regex like `/abc/`

* **Why:** Forces **collection scan** → very slow.

* **Better:** Use indexed fields or full-text search.

# **5\. Use Aggregation Pipeline**

* **What:** Do filtering, grouping, sorting in DB instead of in app code.

* **Why:** Reduces data sent over network, faster processing.

```bash
db.orders.aggregate([
  { $match: { status: "active" } },
  { $group: { _id: "$userId", total: { $sum: "$amount" } } }
]);
```

# **6\. Use Lean Queries (Mongoose)**

* **What:** Skip Mongoose document overhead if you only need raw JSON.

# **7\. Connection Pooling**

* **What:** Reuse DB connections instead of creating new one per request.

* **Why:** Avoids DB overload & reduces latency.

# **Cursor-Based Pagination (a.k.a Keyset Pagination)**

### **✅ What it is:**

Instead of using `skip` \+ `limit` (offset pagination), you **use a unique key (usually \_id or createdAt) as a cursor** to fetch the next set of results.

---

### **Why use it:**

* Offset pagination (`skip`) is slow on large collections → Mongo scans and skips N documents

* Cursor pagination → fast, scalable, and avoids missing/duplicate data if new rows are added

```bash
// First page
const firstPage = await db.posts
  .find({})
  .sort({ _id: 1 })
  .limit(10);

// Next page (cursor = last _id from previous page)
const lastId = firstPage[firstPage.length - 1]._id;
const nextPage = await db.posts
  .find({ _id: { $gt: lastId } })
  .sort({ _id: 1 })
  .limit(10);
```

# **2️⃣ Full Column Index**

### **✅ What it is:**

Creating an **index on a field that’s used frequently in queries** so MongoDB can search **without scanning the whole collection**.

### **Key Points:**

1. **Compound index** – Index on multiple fields if queries filter on multiple fields together:

```bash
db.orders.createIndex({ userId: 1, createdAt: -1 });
```

2. **Covered index** – If the query only uses fields in the index, Mongo doesn’t even read the document (super fast):

```bash
db.users.createIndex({ email: 1, name: 1 });
```

**Query:**

```bash
db.users.find({ email: "a@b.com" }, { email: 1, name: 1 });
```

3. **Full column index** doesn’t mean “index everything automatically” – you choose the fields that matter.

---

### **⚡ Interview Tip:**

* Cursor-based pagination → “efficient paging for large collections”

* Index → “avoid full collection scan”

* Combine them → paginate efficiently using an **indexed field**.

## **Why to choose GraphQl instead of Rest APIs?**

# **1\. Main Problem with REST**

In REST, each endpoint returns **fixed data**, and you often need **multiple API calls**.

# **2\. GraphQL Solution**

GraphQL allows client to request **exactly what it needs in ONE request**:

# **2\. Key Advantages of GraphQL**

## **✅ 1\. No Over-fetching**

REST returns extra data you don’t need.

GraphQL returns only requested fields.

## **✅ 2\. No Under-fetching**

REST requires multiple requests.  
 GraphQL → single request gets everything.

---

## **✅ 3\. Better for frontend flexibility**

Frontend controls response shape.

Very useful in:

* React

* Next.js

* Mobile apps

---

## **✅ 4\. Strongly Typed Schema**

GraphQL schema defines all types.

# **⚡ Interview Example Answer (Best Answer)**

You can say this:

GraphQL allows clients to request exactly the data they need in a single request, avoiding over-fetching and under-fetching problems present in REST APIs. It provides better performance, flexibility, and reduces the number of API calls, which is especially useful for frontend-heavy applications like React and Next.js.

---

# **⚠️ When REST is better**

REST is better when:

* Simple CRUD APIs

* Small applications

* Caching with HTTP

* File uploads

* Public APIs

# **🔥 Senior-level interview tip**

Best answer ending:

REST is simpler and good for basic CRUD, but GraphQL is preferred in complex applications where clients need flexible and efficient data fetching.

 

# **CAP Theorem (Simple Explanation)**

**CAP theorem says a distributed system can only guarantee 2 out of these 3 properties at the same time:**

* **C — Consistency**

* **A — Availability**

* **P — Partition Tolerance**

You must choose any **2**, not all 3\.

# **1️⃣ Consistency (C)**

Every user gets the **same, latest data**.

Example:  
 If you update your profile name, everyone immediately sees the new name.

No old data.

---

# **2️⃣ Availability (A)**

System **always responds**, even if data may not be latest.

Example:  
 You still get a response, but it might show old data.

System never fails.

---

# **3️⃣ Partition Tolerance (P)**

System continues working even if **network between servers breaks**.

Example:  
 Server A and Server B can't communicate, but system still runs.

This is required in distributed systems.

## **CAP Theorem**
- **What is the CAP Theorem?**
   - A distributed system can only provide two out of three: **Consistency**, **Availability**, and **Partition Tolerance**.
   - Most modern DBs prioritize P + (C or A).

## **SQL vs NoSQL**
- **SQL vs NoSQL: When to choose which?**
   - **SQL**: Fixed schema, complex joins, ACID compliance (Banking, ERP).
   - **NoSQL**: Flexible schema, horizontal scaling, high-speed big data (Social media, Real-time feeds).
