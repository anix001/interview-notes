## **JavaScript Related Questions:**

## **1️⃣ What is the difference between `var`, `let`, and `const`?**

# **`var` (Old Way – Avoid in Modern Code)**

### **✅ Scope: Function scoped**

### **✅ Can be re-declared**

### **✅ Can be reassigned**

# **🔹 2️⃣ `let` (Modern Standard)**

### **✅ Block scoped**

### **✅ Can be reassigned**

### **❌ Cannot be re-declared in same scope**

### **⚠ Hoisted but in Temporal Dead Zone (TDZ)**

#  **3️⃣ `const` (Constant Variable)**

### **✅ Block scoped**

### **❌ Cannot be reassigned**

### **❌ Cannot be re-declared**

### **⚠ Must be initialized at declaration**

Temporal Dead Zone is the time between entering a block scope and the execution of the variable’s declaration where the variable exists but cannot be accessed.

## **2️⃣ What is Closure?**

A closure is when a function remembers variables from its outer scope even after the outer function has finished executing.

### Example

```bash
function outer() {
  let count = 0;

  return function inner() {
    count++;
    console.log(count);
  };
}

const fn = outer();
fn(); // 1
fn(); // 2
```

Why does this work?

Because inner() forms a closure over count.

## **3️⃣ What is the difference between `==` and `===`?**


---

# `==` vs `===`

```bash
5 == "5"   // true
5 === "5"  // false

`==` → type coercion  
 `===` → strict comparison

Always prefer `===`.
```

# **✅ What is `this` in JavaScript?**

In JavaScript, the this keyword refers to the context in which a function is executed. It points to the object that is currently calling the function.

1\. Global Context: When used in the global scope (outside of any function or object), this refers to the global object (window in browsers, or global in Node.js). 

 console.log(this); // In a browser, this logs the window object

2\. Object Method: When this is used inside a method of an object, it refers to the object itself.

const person \= { name: 'Alice', greet: function() { console.log(this.name); // 'this' refers to the person object } }; person.greet(); // Logs: Alice 

3\. Constructor Functions: When used in a constructor function (a function that creates new objects), this refers to the newly created object. 


---

# `this` Example

```bash
const person = {
  name: "Alice",
  greet: function () {
    console.log(this.name);
  }
};

person.greet(); // Alice
```

4\. Arrow Functions: Arrow functions do not have their own this. They inherit this from their surrounding lexical context. 

const person \= { name: 'Alice', greet: function() { setTimeout(() \=\> { console.log(this.name); // 'this' refers to the person object }, 1000); } }; person.greet(); // Logs: Alice after 1 second

# **✅ Debouncing vs Throttling (Simple Explanation)**

Both are used to **control how often a function runs** when an event happens many times.

Examples of frequent events:

* typing in search box

* scrolling

* resizing window

* mouse movement

# **✅ 1\. Debouncing**

👉 **Debouncing waits until the user stops doing something.**

The function runs **only after the event stops for a specific time**.

### **Example**

Search bar typing.

User types:

A → An → Ani → Anis → Anish

Without debounce → **5 API calls** ❌  
 With debounce → **1 API call after user stops typing** ✅


---

# Debounce Example

```bash
function debounce(fn, delay) {
  let timer;

  return function (...args) {
    clearTimeout(timer);

    timer = setTimeout(() => {
      fn(...args);
    }, delay);
  };
}
```

# **✅ 2\. Throttling**

👉 **Throttling ensures a function runs only once in a fixed time interval.**

No matter how many times event triggers.

---

### **Example**

Scrolling page.

User scrolls **100 times in 1 second**.

Throttle → function runs maybe **once every 300ms**.

---

# Throttle Example

```bash
function throttle(fn, delay) {
  let lastCall = 0;

  return function (...args) {
    const now = Date.now();

    if (now - lastCall >= delay) {
      lastCall = now;
      fn(...args);
    }
  };
}
```

## **Call vs apply vs bind**

In JavaScript, **`call`**, **`apply`**, and **`bind`** are used to control what **`this`** refers to when a function runs.

---

# Call / Apply / Bind Example

```bash
const person1 = { name: "Anish" };
const person2 = { name: "Rahul" };

function greet(age, city) {
  console.log(this.name + " is " + age + " years old from " + city);
}
```

`greet` uses **this.name**, so we need to decide which object (`person1` or `person2`) should be `this`.

# **1\. call()**

**call() executes the function immediately** and passes arguments **separated by commas**.

```bash
greet.call(person1, 25, "Delhi")

// Output: Anish is 25 years old from Delhi
```

Here:

* `person1` becomes `this`

* arguments passed normally

# **2\. apply()**

**apply() is almost same as call**, but **arguments are passed as an array**.

```bash
greet.apply(person2, [30, "Mumbai"])

// Output: Rahul is 30 years old from Mumbai
```

Difference from `call`:

* arguments inside **array**

# **3\. bind()**

**bind() does NOT run the function immediately.**  
 It **returns a new function** with `this` fixed.

```bash
const greetRahul = greet.bind(person2, 28, "Pune")

greetRahul()

// Output: Rahul is 28 years old from Pune
```

Here:

* `bind` creates a **new function**

* it runs **only when we call it**

## **Shallow Clone vs Deep Clone (JavaScript)**

### **1️⃣ Shallow Clone**

A **shallow clone** copies only the **first level of an object**.  
 If the object contains **nested objects**, the **reference is copied**, not the actual value.

So **both objects share the same nested object in memory**.


---

# Shallow Copy Example

```bash
const user = {
  name: "Anish",
  address: {
    city: "Janakpur"
  }
};

const shallowCopy = { ...user };

shallowCopy.address.city = "Kathmandu";

console.log(user.address.city); // Kathmandu
```

💡 Even though we changed `shallowCopy`, the **original object also changed** because the **nested object reference is shared**.

# **2️⃣ Deep Clone**

A **deep clone** copies **all levels of the object**, including **nested objects**.

The new object has **completely separate memory references**.


---

# Deep Clone Example

```bash
const user = {
  name: "Anish",
  address: {
    city: "Janakpur"
  }
};

const deepCopy = structuredClone(user);

deepCopy.address.city = "Kathmandu";

console.log(user.address.city); // Janakpur
```

💡 Here the original object **does NOT change** because everything was copied fully.

# **Common Ways to Deep Clone**

### **1️⃣ Modern Way (Best)**

```bash
structuredClone(obj)
```

### **2️⃣ Old Way**

```bash
JSON.parse(JSON.stringify(obj))
```

# **Simple Interview Answer**

**Shallow Clone:**  
 Copies only the first level; nested objects share the same reference.

**Deep Clone:**  
 Copies all levels of the object; nested objects are fully independent.

## **Interview One-Line Answer**

**Spread operator performs a shallow copy, meaning it copies only the first level of an object while nested objects remain shared by reference.**

## **Hoisting in JavaScript (Simple Explanation)**

**Hoisting** means JavaScript **moves declarations to the top of their scope before execution**.

So you can sometimes **use variables or functions before they are declared in code**.

# **1️⃣ Function Hoisting**

Function declarations are **fully hoisted**, so you can call them before defining them.

### **Example**

```bash
sayHello()

function sayHello() {  
 console.log("Hello")  
}

// Output: Hello
```

### **What JavaScript internally does**

```bash
function sayHello() {  
 console.log("Hello")  
}

sayHello()
```

So the function is **moved to the top**.

---

# **2️⃣ var Hoisting**

Variables declared with **`var` are hoisted but initialized as `undefined`**.

### **Example**

```bash
console.log(a)

var a = 10

// Output: undefined
```

### **Internally JavaScript sees this**

```bash
var a  
console.log(a)

a = 10
```
---

# **3️⃣ let and const Hoisting**

`let` and `const` are **also hoisted**, but they stay in a **Temporal Dead Zone (TDZ)** until initialization.

So accessing them **before declaration causes an error**.

### **Example**

```bash
console.log(a)

let a = 10

// Output: ReferenceError: Cannot access 'a' before initialization
```
---

# **4️⃣ Function Expression (Important Interview Point)**

Function expressions **are NOT hoisted like function declarations**.

### **Example**

```bash
sayHello()

var sayHello = function () {  
 console.log("Hello")  
}

// Output: TypeError: sayHello is not a function
```

Because JavaScript sees this internally:

```bash
var sayHello

sayHello()   // undefined()

sayHello = function() {  
 console.log("Hello")  
}
```

# **One-Line Interview Answer**

**Hoisting is JavaScript's behavior of moving variable and function declarations to the top of their scope during the compilation phase.**

## 

## **Prototypal Inheritance**

Prototypal inheritance is a way for objects in JavaScript to share properties and methods. Instead of classes and instances, JavaScript uses prototypes.

Basic Concepts

Object Prototype:

Every JavaScript object has a prototype. A prototype is also an object. An object inherits properties and methods from its prototype.

Prototype Chain:

If an object doesn't have a property or method, JavaScript looks for it in the object's prototype. This continues up the chain until it finds the property/method or reaches the end of the chain (usually Object.prototype).

Example

Let's use an example to explain how prototypal inheritance works:

Step 1: Create a Prototype Object

```bash
const animal = {
  eats: true,
  walk() {
    console.log('Animal walks');
  }
};
```

`animal` is an object with a property `eats` and a method `walk`.

Step 2: Create Another Object Inheriting from the Prototype

```bash
const dog = Object.create(animal);
dog.barks = true;
```

`dog` is an object created with `animal` as its prototype. `dog` now has access to `animal`'s properties and methods. `dog` also has its own property `barks`.

Step 3: Access Inherited Properties and Methods

```bash
console.log(dog.eats); // true (inherited from animal)
dog.walk(); // "Animal walks" (method inherited from animal)
console.log(dog.barks); // true (own property)
```

`dog` can access `eats` and `walk` from `animal`. `dog` also has its own property `barks`.

## **Blocking and Non-Blocking in Node**

#### **What is Blocking?**

It refers to the blocking of further operation until the current operation finishes. Blocking methods are executed synchronously. Synchronously means that the program is executed line by line. The program waits until the called function or the operation returns.

Example: Following example uses the readFileSync() function to read files and demonstrate Blocking in Node.js

---

# Node Blocking Example

```bash
const fs = require("fs");

const filepath = "text.txt";

const data = fs.readFileSync(filepath, { encoding: "utf8" });

console.log(data);

// This section calculates the sum of numbers from 1 to 10

let sum = 0;

for(let i=1; i<=10; i++){

	sum = sum + i;

}

// Prints the sum

console.log('Sum: ', sum);
```

#### **What is Non-Blocking ?**

It refers to the program that does not block the execution of further operations. Non-Blocking methods are executed asynchronously. Asynchronously means that the program may not necessarily execute line by line. The program calls the function and move to the next operation and does not wait for it to return.

Example: Following example uses the `readFile()` function to read files and demonstrate Non-Blocking in Node

```bash
const fs = require('fs');

const filepath = 'text.txt';

// Reads a file in a asynchronous and non-blocking way

fs.readFile(filepath, {encoding: 'utf8'}, (err, data) => {

	// Prints the content of file

	console.log(data);

});

// This section calculates the sum of numbers from 1 to 10

let sum = 0;

for(let i=1; i<=10; i++){

	sum = sum + i;

}

// Prints the sum

console.log('Sum: ', sum);
```

### **Currying in JavaScript (Simple Explanation) 😊**

**Currying** means converting a function that takes **multiple arguments** into a **series of functions that each take one argument**.

Instead of passing all arguments at once, you pass them **one by one**.  
**1️⃣ Normal Function (Without Currying)**

```bash
function add(a, b, c) {
 return a + b + c;
}

add(2, 3, 4); // 9
```

Here the function takes **3 arguments at the same time**.

---

## **2️⃣ Curried Version**

```bash
function add(a) {
 return function (b) {
   return function (c) {
     return a + b + c;
   };
 };
}

add(2)(3)(4); // 9
```

**1️⃣ Logging Utility (Very Practical)**

Suppose you are building a logging system.

### **Without Currying**

```bash
function log(level, message) {
 console.log(`[${level}] ${message}`);
}

log("ERROR", "Something failed");

log("INFO", "User logged in");
```

### **With Currying**

```bash
const log = (level) => (message) => {
 console.log(`[${level}] ${message}`);
};

const errorLog = log("ERROR");

const infoLog = log("INFO");

errorLog("Database failed");

infoLog("User logged in");
```

### **Higher-Order Function (HOF) in JavaScript 😊**

A **Higher-Order Function** is a function that does **at least one of these two things**:

1️⃣ **Takes another function as an argument**  
 2️⃣ **Returns another function**

```bash
function greet(message) {
  return function(name) {
    console.log(message + " " + name);
  }
}

const sayHello = greet("Hello");
sayHello("Anish");
```

### **Promise**

A promise is an object which is used to represent the eventual completion (or failure) of an asynchronous function and its resulting value.

A Promise is in one of these states:

**pending**: initial state, neither fulfilled nor rejected.

**fulfilled**: meaning that the operation was completed successfully.

**rejected**: meaning that the operation failed.

How to create a promise in JS: To create a promise, you need to create an instance object using the `Promise` constructor function.

```bash
const promise = new Promise((resolve, reject) => {
  // Condition to resolve or reject the promise
});
```

In promises, `resolve` is a function with an optional parameter representing the resolved value. Also, `reject` is a function with an optional parameter representing the reason why the promise failed.


---

# Promise Example

```bash
const promise = new Promise((resolve, reject) => {
  const num = Math.random();

  if (num >= 0.5) {
    resolve("Promise is fulfilled!");
  } else {
    reject("Promise failed!");
  }
});

promise.then(
  (value) => console.log(value),
  (reason) => console.error(reason)
);

promise.then(handleResolve, handleReject);
```

It is possible to create an immediately resolved promise, and then attach a callback with the `.then()` method. You can also create an immediately rejected promise in the same way too.

```bash
Promise.resolve("Successful").then((result) => console.log(result));
// Output: Successful

Promise.reject("Not successful").then((result) => console.log(result));
// Error: Uncaught (in promise)
```

The error in the rejected promise is because you need to define a separate callback to handle a rejected promise.

```bash
Promise.reject("Not successful").then(
  () => {
    /*Empty Callback if Promise is fulfilled*/
  },
  (reason) => console.error(reason)
);
// Output: Not successful
```

How to Handle Errors in a Promise: To handle errors in Promises, use the `.catch()` method. If anything goes wrong with any of your promises, this method can catch the reason for that error.

```bash
Promise.reject(new Error("Failed")).catch((reason) => console.error(reason));
// Output: Error: Failed
```

How to Handle Many Promises at Once: Here are the available methods that can help us achieve this:

**1️⃣ Promise.all()**  
`Promise.all()` accepts an array of promises and returns a single promise. It resolves when **all** promises in the input array are fulfilled. If any promise is rejected, `Promise.all()` immediately rejects.

```bash
const promise1 = Promise.resolve(`First Promise's Value`);
const promise2 = new Promise((resolve) => setTimeout(resolve, 3000, `Second Promise's Value`));
const promise3 = new Promise((resolve) => setTimeout(resolve, 2000, `Third Promise's Value`));

Promise.all([promise1, promise2, promise3]).then((values) => {
  values.forEach((value) => console.log(value));
});
```

**2️⃣ Promise.race()**  
`Promise.race()` returns a single promise that settles as soon as **any** of the promises in the array settles (either resolved or rejected).

```bash
const promise1 = Promise.resolve(`First Promise's Value`);
const promise2 = new Promise((resolve) => setTimeout(resolve, 3000, `Second Promise's Value`));
const promise3 = new Promise((resolve) => setTimeout(resolve, 2000, `Third Promise's Value`));

Promise.race([promise1, promise2, promise3]).then((value) => {
  console.log(value); // Resolves with the fastest promise
});
```

**3️⃣ Promise.any()**  
`Promise.any()` returns the **first resolved** promise. If all promises are rejected, it returns an `AggregateError`.

```bash
const promise1 = new Promise((resolve) => setTimeout(resolve, 3000, `First Promise's Value`));
const promise2 = new Promise((resolve) => setTimeout(resolve, 2000, `Second Promise's Value`));
const promise3 = Promise.reject(`Third Promise's Value`);

Promise.any([promise1, promise2, promise3]).then(val => console.log(val));
```

If none of the promises resolve:

```bash
const promise1 = new Promise((resolve, reject) => setTimeout(reject, 3000, `First rejection reason`));
const promise2 = new Promise((resolve, reject) => setTimeout(reject, 2000, `Second rejection reason`));
const promise3 = Promise.reject(`Third rejection reason`);

Promise.any([promise1, promise2, promise3]).catch(({ errors }) => console.log(errors));
```

**4️⃣ Promise.allSettled()**  
`Promise.allSettled()` waits for **all** promises to settle (either fulfilled or rejected) and returns an array of objects describing the outcome of each promise.

```bash
const promise1 = new Promise((resolve) => setTimeout(resolve, 3000, `First Promise's Value`));
const promise2 = new Promise((resolve) => setTimeout(resolve, 2000, `Second Promise's Value`));
const promise3 = Promise.reject(`Third Promise's Value`);

Promise.allSettled([promise1, promise2, promise3]).then(console.log);

/* Output:
[
  { status: 'fulfilled', value: "First Promise's Value" },
  { status: 'fulfilled', value: "Second Promise's Value" },
  { status: 'rejected', reason: "Third Promise's Value" }
]
*/
```

### **Memoization in JavaScript (Simple Explanation) 😊**

**Memoization is an optimization technique where we store the result of a function call so that if the same input comes again, we return the stored result instead of recalculating.**

**👉 In simple words:**

**Remember the result of expensive functions and reuse it when the same input occurs.**

---

# **1️⃣ Without Memoization**

```bash
function square(n) {
 console.log("Calculating...");
 return n * n;
}

square(5); // Calculating... → 25
square(5); // Calculating... → 25 (runs again)
```

**Problem:**  
 Even though input is the same, the function recalculates.

---

# **2️⃣ With Memoization**

```bash
function memoizedSquare() {
 const cache = {};
 return function (n) {
   if (cache[n]) {
     console.log("From cache");
     return cache[n];
   }
   console.log("Calculating...");
   const result = n * n;
   cache[n] = result;
   return result;
 };
}

const square = memoizedSquare();
square(5); // Calculating... → 25
square(5); // From cache → 25
```

Now the result is stored in cache.

### **Tree Shaking (Simple Explanation) 🌳**

**Tree Shaking** is a **JavaScript optimization technique** used by bundlers (like Webpack, Rollup, and Vite) to **remove unused code from the final bundle**.

👉 Simple definition:

**Tree shaking removes unused imports from your code so the final bundle size becomes smaller.**

---

# **1️⃣ Example Without Tree Shaking**

Suppose you have a file:

### **`math.js`**

```bash
export function add(a, b) {
 return a + b;
}

export function multiply(a, b) {
 return a * b;
}

export function divide(a, b) {
 return a / b;
}
```

Now in another file:

```bash
import { add } from "./math";

console.log(add(2, 3));
```

You only use **`add()`**.

But without tree shaking, the bundle may include:
- `add`
- `multiply`
- `divide`

Even though **multiply and divide are never used**.

---

# **2️⃣ With Tree Shaking**

The bundler analyzes the imports and removes unused code.

Final bundle contains only:

```bash
function add(a, b) {
 return a + b;
}
```

✅ Smaller bundle  
 ✅ Faster loading

### **Connection Pooling (Simple Explanation)** 

**Connection Pooling** is a technique used in **databases** to **reuse existing connections** instead of creating a new one every time a query runs. This improves **performance and resource management**.

## **Benefits of Connection Pooling**

* Faster query execution

* Limits database overload

* Efficient resource usage

* Reduces connection creation overhead

### **Bundlers vs Transpilers (Simple Explanation)** 

Both are **build tools in JavaScript**, but they do **different jobs**.

---

# **1️⃣ Transpiler**

A **Transpiler** converts **modern JavaScript code into older JavaScript** so that older browsers can understand it.

Example tool: Babel

### **Example**

Modern JS:

```bash
const greet = (name) => `Hello ${name}`;
```

After transpiling (older JS):

```bash
var greet = function(name) {
 return "Hello " + name;
};
```

✅ Same logic  
 ✅ Different syntax for compatibility

**Purpose:**  
 👉 Make modern JavaScript work in **older browsers**

---

# **2️⃣ Bundler**

A **Bundler** combines **multiple files/modules into one or few optimized files** for the browser.

Examples:

* Webpack

* Vite

* Rollup

### **Example**

Project structure:
- `index.js`
- `utils.js`
- `api.js`

Browser cannot easily load many modules efficiently, so bundler creates:

```bash
// bundle.js
// (Containing all code combined and optimized)
```

### **JWKS (JSON Web Key Set) – Simple Explanation** 

**JWKS** stands for **JSON Web Key Set**.

It is a **JSON file that contains public keys used to verify JWT tokens**.

It is commonly used in authentication systems like **Auth0**, **Firebase Authentication**, or **Keycloak**.

---

# **1️⃣ The Problem JWKS Solves**

When using **JWT (JSON Web Tokens)**:

1. Server **creates a JWT** using a **private key**

2. Client sends that JWT to another service

3. That service must **verify the JWT**

But the service **cannot access the private key**.

So it needs the **public key**.

👉 JWKS provides those **public keys**.

## **Event Delegation**
- **What is Event Delegation?**
   - Simple Explanation: Instead of adding event listeners to every single child, you add one listener to the parent. It uses "event bubbling" to catch events from children.
   - Example:
   ```bash
   document.querySelector('#parent').addEventListener('click', (e) => {
     if (e.target.tagName === 'LI') {
       console.log('List item clicked!');
     }
   });
   ```

## **null vs undefined**
- **Difference between `null` and `undefined`?**
   - `undefined`: A variable has been declared but not yet assigned a value.
   - `null`: An intentional assignment of "no value".
   ```bash
   let a; // undefined
   let b = null; // null
   ```