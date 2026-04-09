# 🎨 Design Pattern Interview Questions

## **1️⃣ What is Dependency Injection (DI) and why is it important?**

**Answer:**

**Dependency Injection (DI)** is a pattern where an object receives its dependencies from an outside source rather than creating them itself.

* **Purpose**: Decouple objects and make them more flexible, testable, and maintainable.
* **Benefits**:
    * **Flexibility**: Easily swap out different implementations (e.g., a mock repository for testing).
    * **Maintainability**: Centralize dependency management.
    * **Testability**: Makes unit testing easier by allowing mock objects to be injected.

### **Example (Node.js)**

**Without DI (Tightly Coupled)**:
```javascript
// Service directly imports the model, making it hard to test/replace
import { User } from '../models/user.model.js';

export class UserService {
  async createUser(data) {
    return await User.create(data); 
  }
}
```

**With DI (Decoupled)**:
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

## **2️⃣ What is the MVC (Model-View-Controller) pattern?**

**Answer:**

The **MVC** pattern is a structural design pattern that separates an application into three main components:

1. **Model**: Represents the database logic and schema (e.g., MongoDB, PostgreSQL schemas).
2. **View**: Handles the output or presentation layer (e.g., JSON responses, HTML templates).
3. **Controller**: Contains the business logic, handles user input, and coordinates between the Model and View.

### **Folder Structure Pattern**
```text
src/
├── models/       # Database schemas
├── views/        # UI/Templates (optional for APIs)
├── controllers/  # Route handlers
├── routes/       # Endpoint definitions
├── services/     # Core business logic
└── app.js        # Entry point
```
