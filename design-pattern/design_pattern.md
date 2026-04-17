---
layout: default
title: Design Patterns
nav_order: 10
---

# 🎨 Design Pattern Interview Questions

Common architectural and behavioral patterns used in modern software development.

---

## 📑 Table of Contents
- [MVC (Model-View-Controller)](#-mvc-model-view-controller)
- [Design Pattern Classification](#-design-pattern-classification)

---

## 🏗️ MVC (Model-View-Controller)

The **MVC** pattern is a structural design pattern that separates an application into three main components to achieve a "Separation of Concerns".

1. **Model:** Represents the data layer and business logic (e.g., Database schemas).
2. **View:** The presentation layer (e.g., UI, JSON response).
3. **Controller:** The intermediary that handles user input and coordinates between Model and View.

### Standard Folder Structure
```text
src/
├── models/       # Data schemas (Mongoose/Sequelize)
├── controllers/  # Route handlers & Business logic
├── routes/       # Endpoint definitions
├── services/     # Reusable business logic/External API calls
└── app.js        # Entry point
```

---

## 🧩 Design Pattern Classification

### 1️⃣ Creational Patterns
Focus on object creation mechanisms.
* **Singleton:** Ensures a class has only one instance (e.g., DB connection).
* **Factory:** Creates objects without specifying the exact class of object that will be created.

### 2️⃣ Structural Patterns
Focus on how classes and objects are composed.
* **Adapter:** Allows incompatible interfaces to work together.
* **Proxy:** Provides a placeholder for another object to control access.

### 3️⃣ Behavioral Patterns
Focus on communication between objects.
* **Observer:** A one-to-many dependency where objects are notified of state changes (e.g., Event Listeners).
* **Strategy:** Defines a family of algorithms and makes them interchangeable.

> [!TIP]
> **Interview Gold:** When talking about MVC, emphasize that it makes the code **Testable** and **Maintainable** by decoupling logic from presentation.
