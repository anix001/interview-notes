AWS Related Questions:

# **1️⃣ What is AWS?**

**Answer:**

**Amazon Web Services (AWS)** is a **cloud computing platform** that provides services like servers, storage, databases, networking, and AI over the internet.

Instead of buying physical servers, you can **rent infrastructure on demand**.

Example services:

* Compute → Amazon EC2

* Storage → Amazon S3

* Database → Amazon RDS

# **2️⃣ What is EC2?**

**Amazon EC2 (Elastic Compute Cloud)** is a service that provides **virtual servers in the cloud**.

You use it to:

* host applications

* run backend servers

* deploy APIs

Example:  
 Running a **Node.js backend on an EC2 instance**.

# **3️⃣ What is S3?**

**Amazon S3 (Simple Storage Service)** is an **object storage service** used to store files.

Used for:

* images

* videos

* backups

* static websites

Example:  
 Uploading user profile images to an **S3 bucket**.

# **4️⃣ What is IAM?**

**AWS Identity and Access Management (IAM)** is used to **control access to AWS resources**.

You can create:

* Users

* Roles

* Policies

Example:

* Developer → EC2 access

* Admin → full access

# **5️⃣ What is a VPC?**

**Amazon Virtual Private Cloud (VPC)** is a **private network inside AWS**.

You can:

* define IP ranges

* create public/private subnets

* control security

Think of it like **your own private data center in the cloud**.

# **6️⃣ What is RDS?**

**Amazon RDS (Relational Database Service)** is a **managed relational database service**.

Supported databases:

* MySQL

* PostgreSQL

* MariaDB

* Oracle

* SQL Server

Benefits:

* automatic backups

* scaling

* maintenance handled by AWS

# **7️⃣ What is Lambda?**

\*\*AWS Lambda lets you **run code without managing servers**.

This is called **serverless computing**.

Example:

* Image upload → trigger Lambda → resize image → store in S3.

# **8️⃣ What is CloudFront?**

\*\*Amazon CloudFront is a **Content Delivery Network (CDN)**.

It speeds up websites by delivering content from **servers closer to the user**.

Example:  
 Serving static files globally.

## **Semantic Search vs Hybrid Search (Simple Explanation)**

### **1️⃣ Semantic Search**

**Semantic search** finds results based on the **meaning of the query**, not just exact keywords.

It usually uses **AI embeddings from models like BERT or OpenAI Embeddings**.

### **Example**

Query:

How to fix laptop battery issue

Document:

Troubleshooting problems with notebook power life

A **semantic search engine understands that**:

* *laptop* ≈ *notebook*

* *battery issue* ≈ *power life problem*

So it still returns the result even though **keywords are different**.

📌 It focuses on **intent and meaning**.

### **2️⃣ Hybrid Search**

**Hybrid search combines two methods:**

1. **Keyword search** (traditional search like Elasticsearch BM25)

2. **Semantic search** (vector similarity)

So it uses **both keywords \+ meaning** to rank results.

### **Example**

Query:

best javascript framework

Hybrid search considers:

* Keyword match → “javascript framework”

* Semantic meaning → related concepts like **React, Angular, Vue**

Systems like \*\*Elasticsearch or Pinecone often combine both.

# **Simple Interview Answer**

**Semantic search retrieves results based on the meaning of the query using vector embeddings, while hybrid search combines semantic search with traditional keyword search to improve relevance.**

    **LLM (Large Language Model) — Simple Explanation**

A **LLM (Large Language Model)** is an AI model trained on **huge amounts of text data** to **understand and generate human-like language**.

Examples:

* GPT-4

* ChatGPT

* BERT

* LLaMA

They learn patterns in language so they can **answer questions, write text, summarize, translate, and generate code**.

---

## **Very Simple Way to Think About It**

An **LLM is like an extremely advanced autocomplete system** trained on billions of sentences.

Example:

Input:

The capital of France is

The model predicts the next word:

Paris

It keeps predicting **next words based on probability**.

**What is RAG (Retrieval-Augmented Generation)? — Simple Explanation**

**RAG** means:  
 ➡️ *First retrieve relevant information*  
 ➡️ *Then generate an answer using an LLM*

Think of it like an **open-book exam** for an AI.

### **Simple Flow**

1. User asks a question.

2. System **searches documents/database** for relevant info.

3. Retrieved data is sent to the **LLM**.

4. The **LLM generates a final answer** using that data.

### **Example**

Suppose you build a chatbot for company policies.

**User Question:**

“How many annual leaves are allowed?”

**RAG process:**

1️⃣ Search company documents  
 → finds policy document

2️⃣ Send relevant paragraph to LLM

3️⃣ LLM generates answer:

“Employees are entitled to 18 annual leave days per year.”

So instead of relying only on model training, **RAG uses external knowledge**.

## **1️⃣ What is Prompt Engineering? (Simple Explanation)**

**Prompt engineering** means:

Writing a **clear and structured prompt** so that an **LLM gives the best possible answer**.

A **prompt** is simply the **instruction you give to the AI**.

So:

👉 **Prompt Engineering \= Designing better instructions for AI**

## **Edge Location**
- **What is an Edge Location?**
   - Small data centers used by **CloudFront** (CDN) to cache your content closer to the users, reducing latency.


## **Load Balancer vs API Gateway?**

**Answer:**

* **Load Balancer (LB)**:
    * **Purpose**: Distributes incoming network traffic across multiple backend servers to ensure no single server is overloaded.
    * **Focus**: Availability and reliability at the network/transport layer (L4 or L7).
* **API Gateway**:
    * **Purpose**: A single entry point for all clients. It sits in front of microservices.
    * **Focus**: High-level concerns like Authentication, Authorization, Rate Limiting, Request Transformation, and Protocol Translation (e.g., REST to gRPC).
* **Key Difference**: A Load Balancer just routes traffic; an API Gateway "manages" the API requests with business logic (security, logging, etc.).




##  **Horizontal vs Vertical Scaling?**

**Answer:**

* **Vertical Scaling (Scale Up)**:
    * Adding more power (CPU, RAM) to an existing server.
    * **Limit**: Eventually, you hit the hardware ceiling of a single machine.
* **Horizontal Scaling (Scale Out)**:
    * Adding more servers to the resource pool.
    * **Benefit**: Practically limitless scaling and better fault tolerance.
    * **Requirement**: Requires a Load Balancer and stateless application design.
