---
layout: default
title: Artificial Intelligence
nav_order: 5
---

# 🤖 Artificial Intelligence Interview Questions

A comprehensive guide to modern AI concepts, including Agentic AI, MCP, and Retrieval-Augmented Generation (RAG).

---

## 📑 Table of Contents
- [🔌 Model Context Protocol (MCP)](#-model-context-protocol-mcp)
- [🤖 Agentic AI](#-agentic-ai)
- [🔎 Semantic Search vs Hybrid Search](#-semantic-search-vs-hybrid-search)
- [🧠 LLM (Large Language Model)](#-llm-large-language-model)
- [📖 What is RAG (Retrieval-Augmented Generation)?](#-what-is-rag-retrieval-augmented-generation)
- [✍️ What is Prompt Engineering?](#-what-is-prompt-engineering)

---

## 🔌 Model Context Protocol (MCP)

Model Context Protocol is a **standardized interface** for AI models to access external data and tools securely and consistently.

### 🧠 The Core Idea
Instead of custom, messy integrations for every application, MCP provides a "plug-and-play" uniform interface for:
* Databases
* APIs
* Filesystems
* Internal Services

### 🧩 Architecture
1. **Host (LLM App):** The application using the LLM (e.g., a chatbot or IDE).
2. **MCP Server:** The adapter layer that standardizes tool definitions and execution.
3. **Tools/Resources:** The actual assets being accessed (DB, API, etc.).

> [!TIP]
> **Interview Gold:** Think of MCP as the "USB port" for AI. It allows any model to connect to any data source using a single standard.

**Concrete example — without MCP vs with MCP:**
```
WITHOUT MCP (custom integration per app):
ChatBot App → custom code → connects to PostgreSQL
ChatBot App → different custom code → connects to Slack
ChatBot App → yet more custom code → connects to GitHub
(Every integration is one-off, fragile, hard to maintain)

WITH MCP (standardized):
ChatBot App → MCP Client → MCP Server (PostgreSQL adapter)
ChatBot App → MCP Client → MCP Server (Slack adapter)
ChatBot App → MCP Client → MCP Server (GitHub adapter)
(Same protocol for all tools — write the client once, plug in any server)
```

**Real use case:** A coding assistant (like Claude in VS Code) uses MCP to read your codebase, run terminal commands, and access documentation — all through standardized tool calls, not custom plugins for each IDE.

---

## 🤖 Agentic AI

Agentic AI refers to systems built around an LLM that can **plan, reason, and take actions** autonomously to achieve a goal.

### 🔄 The Workflow
1. **Plan:** Break complex tasks into steps.
2. **Execute:** Use tools (APIs, DBs, browsers) to perform actions.
3. **Reason:** Analyze results and iterate if errors occur.
4. **Remember:** Maintain state across long-running tasks.

**Concrete example — "Book the cheapest flight to Mumbai next Friday":**
```
User: "Book the cheapest flight to Mumbai next Friday"
       ↓
Agent Plans:
  Step 1: Search flights → calls flight search API tool
  Step 2: Compare prices → reasons about results
  Step 3: Select cheapest → "IndiGo at ₹3,200"
  Step 4: Book it → calls booking API tool with card details
  Step 5: Email confirmation → calls email tool
       ↓
Agent: "Done! Booked IndiGo 6E-204, ₹3,200. Confirmation sent to your email."
```

The key difference from a regular chatbot: **the agent takes real actions in the world**, not just generates text.

---

## 🔎 Semantic Search vs Hybrid Search

### **1️⃣ Semantic Search**

**Semantic search** finds results based on the **meaning of the query**, not just exact keywords. It usually uses **AI embeddings from models like BERT or OpenAI Embeddings**.

#### **Example**
* **Query:** "How to fix laptop battery issue"
* **Document:** "Troubleshooting problems with notebook power life"

A **semantic search engine understands that**:
* *laptop* ≈ *notebook*
* *battery issue* ≈ *power life problem*

So it still returns the result even though **keywords are different**.

📌 It focuses on **intent and meaning**.

### **2️⃣ Hybrid Search**

**Hybrid search combines two methods:**
1. **Keyword search** (traditional search like Elasticsearch BM25)
2. **Semantic search** (vector similarity)

So it uses **both keywords + meaning** to rank results.

#### **Example**
* **Query:** "best javascript framework"
* **Hybrid search considers:**
    * Keyword match → “javascript framework”
    * Semantic meaning → related concepts like **React, Angular, Vue**

Systems like **Elasticsearch or Pinecone** often combine both.

> [!IMPORTANT]
> **Simple Interview Answer:** Semantic search retrieves results based on the meaning of the query using vector embeddings, while hybrid search combines semantic search with traditional keyword search to improve relevance.

---

## 🧠 LLM (Large Language Model)

A **LLM (Large Language Model)** is an AI model trained on **huge amounts of text data** to **understand and generate human-like language**.

Examples:
* GPT-4, ChatGPT, BERT, LLaMA

They learn patterns in language so they can **answer questions, write text, summarize, translate, and generate code**.

> [!NOTE]
> **Very Simple Way to Think About It:** An **LLM is like an extremely advanced autocomplete system** trained on billions of sentences.

**How training works (simplified):**
1. Feed the model trillions of words from books, websites, and code.
2. The model learns patterns: "after the word 'The sky is', the word 'blue' is very likely."
3. Scale this up with billions of parameters (weights) and the model learns grammar, facts, reasoning, and style.
4. Fine-tune with human feedback (RLHF) to make responses helpful and safe.

**Why "large"?** GPT-4 has ~1.8 trillion parameters. Each parameter is a number that was adjusted during training to capture a pattern in language. More parameters = more nuanced understanding.

**What LLMs can't do:** They don't "know" things the way humans do — they predict statistically likely text. They can hallucinate (confidently state false facts) because they optimize for plausible text, not verified truth. That's why RAG and tool use are important.

---

## 📖 What is RAG (Retrieval-Augmented Generation)?

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
* **User Question:** “How many annual leaves are allowed?”
* **RAG process:**
    1. Search company documents → finds policy document
    2. Send relevant paragraph to LLM
    3. LLM generates answer: “Employees are entitled to 18 annual leave days per year.”

So instead of relying only on model training, **RAG uses external knowledge**.

---

## ✍️ What is Prompt Engineering?

**Prompt engineering** means designing **clear and structured instructions** (prompts) so that an **LLM gives the best possible answer**.

👉 **Prompt Engineering = Designing better instructions for AI**

### Key Techniques:
* **Few-Shot Prompting:** Providing examples in the prompt.
* **Chain of Thought:** Asking the model to "think step-by-step".
* **Persona Adoption:** Telling the AI to "act as a Senior Software Engineer".

**Before/after examples showing the difference:**

**Zero-Shot (no examples):**
```
Prompt: "Classify this review as positive or negative: 'The product broke on day one.'"
Result: "Negative" ✅ (works for simple tasks)
```

**Few-Shot (with examples — better for complex tasks):**
```
Prompt:
"Classify sentiment:
 Review: 'Great quality!' → Positive
 Review: 'Arrived damaged.' → Negative
 Review: 'Decent but overpriced.' → ?"
Result: "Neutral" ✅ (model learned the pattern from examples)
```

**Chain of Thought (forces step-by-step reasoning — great for math/logic):**
```
❌ Without CoT:
Prompt: "If there are 5 birds and 3 fly away, how many remain?"
Result: "2" ✅ (works here but fails on complex multi-step problems)

✅ With CoT:
Prompt: "If there are 5 birds and 3 fly away, how many remain? Think step by step."
Result: "There are 5 birds. 3 fly away. 5 - 3 = 2. There are 2 birds remaining." ✅
```

> [!TIP]
> **Interview Gold:** Chain of Thought prompting reduces hallucination on reasoning tasks by forcing the model to show its work. If the reasoning is visible, errors are easier to catch and correct.

---

## 🗄️ What are Vector Databases?

A **vector database** stores data as **mathematical vectors** (arrays of numbers called embeddings) and lets you search by **meaning/similarity** rather than exact keyword match.

* **Normal DB query:** `WHERE name = 'dog'` — exact match.
* **Vector DB query:** "Find me things similar to this image/sentence" — returns nearest neighbours.

### **How it works**
1. Text or images are converted to vectors (embeddings) by an AI model.
2. Vectors are stored in the database.
3. At query time, your query is also converted to a vector.
4. The DB finds the closest vectors using distance algorithms (cosine similarity, dot product).

### **Popular vector DBs**
* Pinecone, Weaviate, Qdrant, pgvector (Postgres extension), ChromaDB.

> [!TIP]
> **Interview Gold:** Vector databases are the "memory" layer of RAG systems. Instead of searching documents by keyword, you search by semantic meaning — so "automobile" and "car" are treated as similar even though the words are different.

---

## 🔁 Fine-tuning vs RAG — When to Use Which?

Both approaches improve LLM output quality, but they solve different problems.

### **RAG (Retrieval-Augmented Generation)**
* Keep the base LLM unchanged. At query time, retrieve relevant documents and inject them into the prompt.
* ✅ Always up-to-date (just update the document store).
* ✅ Cheaper and faster to set up.
* Use when: you want the model to answer questions about **external or private data** (company docs, product manuals).

### **Fine-tuning**
* Train the model further on your own dataset to change its **style, tone, or domain knowledge**.
* ✅ The model "learns" a specific format, writing style, or terminology.
* ❌ Expensive, requires labelled data, knowledge can go stale.
* Use when: you need consistent output style, specialized terminology, or the model needs to behave very differently from the base model.

> [!TIP]
> **Interview Gold:** **Start with RAG, not fine-tuning.** RAG is faster, cheaper, and easier to keep updated. Fine-tune only when RAG's output quality isn't good enough after prompt engineering.

---

## 🔢 What are Embeddings?

An **embedding** is a vector (array of numbers) that represents the **meaning** of text, images, or other data. Similar meanings produce vectors that are mathematically close to each other.

```
"king"   → [0.2, 0.8, 0.1, ...]
"queen"  → [0.2, 0.7, 0.3, ...]   ← close to "king"
"banana" → [0.9, 0.1, 0.5, ...]   ← far from "king"
```

### **How they're used**
* **Semantic search:** Find documents similar in meaning to a query.
* **Recommendation systems:** "Users who liked X also liked Y" (X and Y have similar embedding vectors).
* **RAG:** Convert your documents into embeddings and store in a vector DB for retrieval.

### **How to generate them**
Use an embedding model (OpenAI `text-embedding-ada-002`, Cohere, or open-source `sentence-transformers`).

```javascript
// OpenAI SDK example
const response = await openai.embeddings.create({
  model: "text-embedding-ada-002",
  input: "What is the return policy?",
});
const vector = response.data[0].embedding; // array of 1536 numbers
```

> [!TIP]
> **Interview Gold:** Embeddings turn language into math. Once you have that, you can do similarity search, clustering, and classification — things traditional keyword search can't do. This is why vector databases and embeddings are the backbone of modern AI-powered search.
