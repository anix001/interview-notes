---
layout: default
title: Artificial Intelligence
nav_order: 5
---

# 🤖 Artificial Intelligence Interview Questions

A comprehensive guide to modern AI concepts, including Agentic AI, MCP, and Retrieval-Augmented Generation (RAG).

---

## 📑 Table of Contents
- [Model Context Protocol (MCP)](#-model-context-protocol-mcp)
- [Agentic AI](#-agentic-ai)
- [Semantic vs Hybrid Search](#-semantic-vs-hybrid-search)
- [LLM Fundamentals](#-llm-fundamentals)
- [Retrieval-Augmented Generation (RAG)](#-retrieval-augmented-generation-rag)
- [Prompt Engineering](#-prompt-engineering)

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

### 🔥 Key Differences
| Aspect | Standard LLM | Agentic AI |
| :--- | :--- | :--- |
| **Role** | Text generator | Decision-making system |
| **Behavior** | Reactive | Proactive |
| **Tools** | None by default | Integrated tool-use |
| **Goal Execution** | No | Yes |

---

## 🔎 Semantic vs Hybrid Search

### 1️⃣ Semantic Search
Focuses on the **meaning and intent** of a query rather than exact keyword matches. It uses **vector embeddings** to find related concepts.
* *Example:* "laptop battery issue" matches "notebook power life problems".

### 2️⃣ Hybrid Search
Combines **Keyword Search** (BM25/Elasticsearch) with **Semantic Search** (Vectors) to provide the most relevant results.
* *Example:* "best javascript framework" uses keyword matching for "javascript" and semantic meaning for "React, Angular, Vue".

---

## 🧠 LLM Fundamentals

Large Language Models (LLMs) are AI models trained on massive datasets to understand and generate human-like language by predicting the **next most probable token**.

> [!NOTE]
> An LLM is essentially an extremely advanced autocomplete system trained on billions of sentences.

---

## 📖 Retrieval-Augmented Generation (RAG)

RAG is a technique that gives LLMs access to **external, real-time data** without retraining the model.

### 🔄 Simple Flow
1. **Retrieve:** Search a private database for relevant context.
2. **Augment:** Combine the retrieved info with the user's prompt.
3. **Generate:** The LLM generates an answer based on the provided context.

> [!IMPORTANT]
> RAG solves the problem of "Hallucination" by grounding the AI's responses in verifiable data.

---

## ✍️ Prompt Engineering

Prompt Engineering is the practice of designing **clear, structured instructions** to get the best possible output from an LLM.

### Key Techniques:
* **Few-Shot Prompting:** Providing examples in the prompt.
* **Chain of Thought:** Asking the model to "think step-by-step".
* **Persona Adoption:** Telling the AI to "act as a Senior Software Engineer".
