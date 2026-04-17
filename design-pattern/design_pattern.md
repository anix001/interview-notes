---
layout: default
title: Design Patterns
nav_order: 10
---

---
layout: default
title: Design Patterns
nav_order: 10
---

# 🎨 Design Pattern Interview Questions

Structural and behavioral design patterns for clean, maintainable, and testable code.

---

## 📑 Table of Contents
- [1️⃣ Dependency Injection (DI)](#1️⃣-dependency-injection-di)
- [2️⃣ MVC (Model-View-Controller)](#2️⃣-mvc-model-view-controller)

---

## **1️⃣ What is Dependency Injection (DI) and why is it important?**

**Answer:**

**Dependency Injection (DI)** is a pattern where an object receives its dependencies from an outside source rather than creating them itself.

* **Purpose**: Decouple objects and make them more flexible, testable, and maintainable.
* **Benefits**:
    * **Flexibility**: Easily swap out different implementations (e.g., a mock repository for testing).
    * **Maintainability**: Centralize dependency management.
    * **Testability**: Makes unit testing easier by allowing mock objects to be injected.

### **Example (Node.js)**

#### **Without DI (Tightly Coupled)**:
```javascript
// Service directly imports the model, making it hard to test/replace
import { User } from '../models/user.model.js';

export class UserService {
  async createUser(data) {
    return await User.create(data); 
  }
}
```

#### **With DI (Decoupled)**:
```javascript
// Step 1: Inject the repository via constructor
export class UserService {
  constructor(userRepository) {
    this.userRepository = userRepository;
  }

  async createUser(data) {
    return await this.userRepository.create(data);
  }
}

// Step 2: Pass dependency when initializing
const userService = new UserService(userRepository);
```

---

## **2️⃣ What is the MVC (Model-View-Controller) pattern?**

**Answer:**

The **MVC** pattern is a structural design pattern that separates an application into three main components to achieve a "Separation of Concerns".

1. **Model:** Represents the data layer and business logic (e.g., Database schemas).
2. **View:** The presentation layer (e.g., UI, JSON response).
3. **Controller:** The intermediary that handles user input and coordinates between Model and View.

### **Recommended Folder Structure**
```text
src/
├── models/       # Data schemas (Mongoose/Sequelize)
├── controllers/  # Route handlers & Business logic
├── routes/       # Endpoint definitions
├── services/     # Reusable business logic/External API calls
└── app.js        # Entry point
```

> [!TIP]
> **Interview Gold:** Mentioning the separation of **Services** from **Controllers** shows a higher level of understanding, where Controllers handle the request/response and Services handle the core business logic.
