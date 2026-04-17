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

---

## 🤖 Agentic AI

Agentic AI refers to systems built around an LLM that can **plan, reason, and take actions** autonomously to achieve a goal.

### 🔄 The Workflow
1. **Plan:** Break complex tasks into steps.
2. **Execute:** Use tools (APIs, DBs, browsers) to perform actions.
3. **Reason:** Analyze results and iterate if errors occur.
4. **Remember:** Maintain state across long-running tasks.

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
