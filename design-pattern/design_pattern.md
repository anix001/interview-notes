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

---

## **3️⃣ Singleton Pattern**

Ensures a class has **only one instance** throughout the entire application, and provides a global access point to it.

* Use for: database connection pool, config manager, logger — things that should only exist once.

```javascript
class DatabaseConnection {
  static instance = null;

  constructor() {
    if (DatabaseConnection.instance) {
      return DatabaseConnection.instance; // return existing
    }
    this.connection = createConnection(); // create only once
    DatabaseConnection.instance = this;
  }
}

const db1 = new DatabaseConnection();
const db2 = new DatabaseConnection();
console.log(db1 === db2); // true — same instance
```

> [!TIP]
> **Interview Gold:** In Node.js, you often get Singleton behaviour for free — `require()` caches modules. Once a module is loaded, the same object is returned every time you `require` it.

---

## **4️⃣ Observer Pattern**

Defines a **one-to-many** relationship: when one object (the subject) changes state, all its dependents (observers) are automatically notified.

* Also called **Pub/Sub** (publish/subscribe).
* Use for: event systems, real-time notifications, state management.

```javascript
class EventEmitter {
  constructor() {
    this.listeners = {};
  }

  on(event, callback) {
    if (!this.listeners[event]) this.listeners[event] = [];
    this.listeners[event].push(callback);
  }

  emit(event, data) {
    (this.listeners[event] || []).forEach(cb => cb(data));
  }
}

const emitter = new EventEmitter();
emitter.on("userSignedUp", (user) => console.log("Send welcome email to", user.email));
emitter.on("userSignedUp", (user) => console.log("Add to analytics", user.id));

emitter.emit("userSignedUp", { id: 1, email: "alice@example.com" });
// Both callbacks run automatically
```

> [!TIP]
> **Interview Gold:** Node.js's `EventEmitter`, browser DOM events, and React's state updates are all built on the Observer pattern. It's the foundation of reactive programming.

---

## **5️⃣ Factory Pattern**

A **Factory** is a function or class that creates objects for you. Instead of using `new` directly everywhere, you call the factory and it decides which type of object to create.

* Hides the complexity of object creation.
* Makes it easy to swap implementations without changing the calling code.

```javascript
// Without Factory — calling code must know all types
if (type === "email") new EmailNotifier(config);
else if (type === "sms") new SmsNotifier(config);
else if (type === "push") new PushNotifier(config);

// With Factory — calling code is simple
function createNotifier(type, config) {
  const notifiers = {
    email: EmailNotifier,
    sms: SmsNotifier,
    push: PushNotifier,
  };
  const Notifier = notifiers[type];
  if (!Notifier) throw new Error(`Unknown notifier: ${type}`);
  return new Notifier(config);
}

const notifier = createNotifier("email", { to: "alice@example.com" });
notifier.send("Welcome!");
```

> [!TIP]
> **Interview Gold:** The Factory pattern is the answer to "how do you create objects without tightly coupling your code to specific classes?" Use it whenever the type of object to create is determined at runtime.

---

## **6️⃣ Repository Pattern**

The **Repository** pattern separates your data access logic from your business logic. Instead of writing database queries directly in your service or controller, you put them behind a repository interface.

```javascript
// Without Repository — business logic mixed with DB queries
async function getUserById(id) {
  return await db.query("SELECT * FROM users WHERE id = ?", [id]); // DB leak
}

// With Repository
class UserRepository {
  async findById(id) {
    return await db.query("SELECT * FROM users WHERE id = ?", [id]);
  }

  async save(user) {
    return await db.query("INSERT INTO users SET ?", [user]);
  }
}

class UserService {
  constructor(userRepo) {
    this.userRepo = userRepo; // injected (DI)
  }

  async getUser(id) {
    const user = await this.userRepo.findById(id);
    if (!user) throw new Error("User not found");
    return user;
  }
}
```

### **Why it matters**
* Swap databases (PostgreSQL → MongoDB) by only changing the repository — the service stays the same.
* Easy to mock in tests (inject a fake repository).

> [!TIP]
> **Interview Gold:** Repository + Dependency Injection together are the two patterns that make code **testable and swappable**. If you use these two, you'll impress any senior interviewer asking about clean architecture.
