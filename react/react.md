## **REACT JS QUESTIONS:**

## **✅ Q1: What is React?**

**Answer:**

React is a JavaScript library for building user interfaces using reusable components and a virtual DOM for efficient updates.

## **✅ Q2: What is Virtual DOM?**

**Answer:**

Virtual DOM is a lightweight copy of the real DOM. React compares virtual DOM with previous version and updates only changed parts in real DOM.

**Benefit:** Faster performance.

## **✅ Q3: What is a component?**

**Answer:**

A component is a reusable piece of UI.

## **✅ Q4: What is useState?**

Used to manage state in functional components.

## **✅ Q5: What is useEffect?**

Used for side effects like:

* API calls

* Event listeners

* Timers

## **✅ Q7: What is the difference between state and props?**

| State | Props |
| ----- | ----- |
| Managed inside component | Passed from parent |
| Can change | Read-only |
| Mutable | Immutable |

## **✅ Q8: What is React.memo?**

Prevents unnecessary re-renders.

```bash
export default React.memo(User);
```

Used when props don’t change.

## **✅ Q9: What is useMemo?**

Memoizes calculated value.

```bash
const result = useMemo(() => expensiveCalc(data), [data]);
```

Prevents recalculation.

## **✅ Q10: What is useCallback?**

Memoizes function.

```bash
const handleClick = useCallback(() => {
  console.log("clicked");
}, []);
```

Prevents function recreation.

# **🔥 5\. Rendering Questions (Very common)**

## **✅ Q11: When does React component re-render?**

Re-renders when:

* State changes

* Props changes

* Parent re-renders

## **✅ Q12: Difference between controlled and uncontrolled components**

Controlled:

```bash
<input value={name} onChange={e => setName(e.target.value)} />
```

React controls input.

Uncontrolled:

```bash
<input ref={inputRef} />
```

DOM controls input.

---

## **✅ Q14: How do you prevent unnecessary re-renders?**

Answer:

* React.memo

* useMemo

* useCallback

* Proper state management

# **🧠 7\. Important Concept Questions**

## **✅ Q15: What is lifting state up?**

Moving state to parent component to share between children.

## **✅ Q16: What is key in React?**

Key helps React identify which items changed.

```bash
{users.map(user => (
  <div key={user.id}>{user.name}</div>
))}
```

## **✅ Q17: Difference between useRef and useState**

useState:

* Causes re-render

useRef:

* Does NOT cause re-render

Example:

```bash
const ref = useRef(0);
```

## **✅ Q18: What is reconciliation?**

Process where React compares virtual DOM and updates real DOM efficiently.

## **✅ Q19: What is prop drilling?**

Passing props through many levels.

Solution:

* Context API

* Zustand

* Redux

# **🚀 9\. Next.js Specific React Questions (Important for YOU)**

## **✅ Q20: Difference between CSR and SSR**

CSR → Client Side Rendering  
 SSR → Server Side Rendering

Next.js supports both.

# **🏆 Senior-Level Answer (Best One)**

If interviewer asks:

“How would you optimize a slow React app?”

Say:

First, I would use React DevTools Profiler to detect unnecessary re-renders. Then I would optimize using React.memo, useMemo, and useCallback where needed. I would ensure proper key usage, implement lazy loading, virtualize large lists, and minimize state updates.

# **Common Mistake (Very Important)**

Overusing useMemo/useCallback ❌

They also have cost.

Use only when:

* Expensive computation

* Child component wrapped in React.memo

## **Why does React re-render even when state looks same?**

You say:

React compares state and props using shallow comparison. For objects and arrays, even if the values look identical, a new reference causes React to re-render. React does not perform deep comparison for performance reasons.

That answer sounds senior-level.

# **🎯 What is React Lifecycle?**

Lifecycle means:

Different stages a component goes through from creation to removal.

Like human life:

Birth → Growth → Death

In React:

Mount → Update → Unmount

#  **1️⃣ Mounting (Component is created)**

This happens when component first appears on screen.

Example:

```bash
useEffect(() => {
  console.log("Component mounted");
}, []);
```

Empty dependency array → runs once after first render.

Use cases:

* API call

* Setup event listener

* Start timer

#  **2️⃣ Updating (Component re-renders)**

Happens when:

* State changes

* Props change

* Parent re-renders

Example:

```bash
useEffect(() => {
  console.log("Count changed");
}, [count]);
```

Runs when `count` changes.

# **3️⃣ Unmounting (Component removed)**

Happens when component disappears.

Example:

```bash
useEffect(() => {
  const interval = setInterval(() => {
    console.log("running");
  }, 1000);
  return () => {
    clearInterval(interval);
    console.log("Component unmounted");
  };
}, []);
```

The return function = cleanup function.

Use cases:

* Clear timers

* Remove event listeners

* Cancel API requests

# **What is Lazy Loading?**

Lazy loading means:

Load components or code only when needed instead of loading everything at once.

This improves:

* Initial load time 🚀

* Bundle size 📦

* Performance ⚡

# **1️⃣ Lazy Loading in React (React.lazy)**

Used to split code and load components dynamically.

### **Example:**

```bash
import React, { Suspense } from "react";
const Dashboard = React.lazy(() => import("./Dashboard"));

function App() {
  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Dashboard />
    </Suspense>
  );
}
```

### **How it works:**

* `Dashboard` is loaded only when rendered.

* `Suspense` shows fallback while loading.

# **2️⃣ Lazy Loading in Next.js**

Next.js has built-in dynamic import.

Use `next/dynamic`.

### **Example:**

```bash
import dynamic from "next/dynamic";

const Dashboard = dynamic(() => import("./Dashboard"), {
  loading: () => <p>Loading...</p>,
});

export default function Page() {
  return <Dashboard />;
}
```

#  **Best Interview Answer (Short Version)**

If interviewer asks:

“What is lazy loading?”

You say:

Lazy loading is a technique where components are loaded only when needed instead of during initial page load. In React, we use React.lazy with Suspense, and in Next.js, we use next/dynamic to split code and improve performance.

##  **How lazy loading affects SEO**

Lazy loading can affect SEO if important content is not rendered on the server. If search engines cannot see the content during initial page load, it may not be indexed properly.

---

# **🧠 Why SEO Can Be Affected**

Search engines (like **Google**) crawl the **HTML generated on first load**.

If:

* Content is rendered via SSR → ✅ Good for SEO

* Content is loaded later via JavaScript → ⚠️ Risky

Because:

Some crawlers may not execute JavaScript fully.

#  **What is Hydration?**

**Hydration** is the process where **React “activates” the server-rendered HTML** on the client so it becomes interactive.

* Next.js renders the page **on the server** (SSR/SSG) → sends HTML to browser

* Browser displays the HTML immediately → fast first paint

* React then **hydrates** the HTML → attaches event listeners, makes buttons clickable, forms work, etc.

---

# **🧠 Key Idea**

Think of it like:

1. Server sends a **static page** → user sees content instantly

2. Client runs React → adds **interactivity** → page behaves like normal SPA

Without hydration → page looks fine, but **buttons and events don’t work**.

# **What is Bundle Optimization?**

Bundle optimization is the process of **reducing the size of JavaScript, CSS, and other assets** sent to the browser, so your app loads faster and performs better.

# **1️⃣ Code Splitting / Lazy Loading**

* Split your bundle into smaller chunks

* Load components only when needed

**Next.js Example:**

```bash
import dynamic from "next/dynamic";

const Dashboard = dynamic(() => import("./Dashboard"), {
  loading: () => <p>Loading...</p>,
});
```

* Dashboard is in a **separate chunk** → only loaded when needed

# **2️⃣ Tree Shaking**

* Remove **unused code** automatically

* Works with ES6 modules (`import { something } from 'lib'`)

Example:

```bash
import { map } from 'lodash'; // only map is included
```

* Without tree shaking, entire lodash might get included → bigger bundle

#  **What is Tree Shaking?**

Tree shaking is a process that removes unused JavaScript code from the final bundle.

It helps reduce bundle size and improve performance.

---

# **🧠 Why is it called “Tree Shaking”?**

Imagine your code like a tree 🌳

* You only use some branches (functions)

* The unused branches are “shaken off”

Final bundle only contains what you use.

#  **What is React Lifecycle?**

React lifecycle refers to the different stages a component goes through from creation to removal.

Think of it like:

**Birth → Life → Death**

In React:

**Mount → Update → Unmount**

# **1️⃣ Mounting (Component is created)**

This happens when the component appears on the screen for the first time.

### **In Functional Components:**

```bash
useEffect(() => {
  console.log("Component Mounted");
}, []);
```

* Empty dependency array `[]`

* Runs only once after first render

Common use cases:

* API calls

* Setting up event listeners

* Starting timers

# **2️⃣ Updating (Component re-renders)**

Happens when:

* State changes

* Props change

* Parent re-renders

Example:

```bash
useEffect(() => {
  console.log("Count changed");
}, [count]);
```

Runs only when `count` changes.

# **3️⃣ Unmounting (Component is removed)**

When component disappears from screen.

Example:

```bash
useEffect(() => {
  const timer = setInterval(() => {
    console.log("Running...");
  }, 1000);
  return () => {
    clearInterval(timer);
    console.log("Component Unmounted");
  };
}, []);
```

The return function = cleanup function.

Used for:

* Clearing intervals

* Removing event listeners

* Canceling API requests

## **React 19 hooks**

# **`use()`**

### **✅ What it does:**

Allows you to directly use Promises (or context) inside a component.

```bash
const data = use(fetchData());
```

### **🔥 Why important?**

* React suspends automatically until promise resolves

* No need for `useEffect + useState`

* Works great with Suspense

### **🧠 Interview line:**

use() lets you read async resources directly during rendering and integrates with Suspense.

```bash
async function fetchData() {
  const res = await fetch("https://jsonplaceholder.typicode.com/posts/1");
  if (!res.ok) {
    throw new Error("Failed to fetch data");
  }
  return res.json();
}

import { use } from "react";

export default function Page() {
  const data = use(fetchData());
  return <h1>{data.title}</h1>;
}
```

# **2️⃣ `useFormStatus()`**

Used inside forms (especially with Server Actions).

```bash
const { pending } = useFormStatus();
```

### **✅ Gives:**

* `pending` (loading state)

* submission status

No need to manually manage loading state.

### **1️⃣ Server Action**

```bash
// app/actions.js
export async function submitForm(formData) {
  "use server";
  const name = formData.get("name");
  // simulate delay
  await new Promise((res) => setTimeout(res, 2000));
  console.log(name);
}
```

---

### **2️⃣ Submit Button Component**

```bash
"use client";
import { useFormStatus } from "react-dom";

function SubmitButton() {
  const { pending } = useFormStatus();
  return (
    <button type="submit" disabled={pending}>
      {pending ? "Submitting..." : "Submit"}
    </button>
  );
}

export default SubmitButton;
```

---

### **3️⃣ Form Component**

```bash
import { submitForm } from "./actions";
import SubmitButton from "./SubmitButton";

export default function Page() {
  return (
    <form action={submitForm}>
      <input name="name" placeholder="Enter name" />
      <SubmitButton />
    </form>
  );
}
```

# **3️⃣ `useFormState()`**

Helps manage form results from server actions.

```bash
const [state, formAction] = useFormState(action, initialState);
```

### **✅ Used for:**

* Server-side validation errors

* Returning structured form state

Cleaner than managing error state manually.

---

# **4️⃣ `useOptimistic()`**

Helps implement **optimistic UI updates**.

```bash
const [optimisticState, addOptimistic] = useOptimistic(
  state,
  (current, newValue) => [...current, newValue]
);
```

### **🔥 Example:**

User submits a comment → UI updates instantly → server confirms later.

Very useful in:

* Chat apps

* Like buttons

* Comments

* Notifications

---

# **5️⃣ `useActionState()` (Replaces some `useFormState` patterns)**

Helps manage async action state more cleanly.

A hook that helps manage async action state (like form submission) in a clean and structured way.

It replaces many old patterns like:

* `useState` for loading

* `useState` for errors

* manual try/catch handling

* complex form logic

```bash
const [state, action, isPending] = useActionState(myAction, initialState);
```

Gives:

* current state

* action function

* loading boolean

 

## **1️⃣ Server Action**

```bash
// actions.js
export async function createUser(prevState, formData) {
  "use server";
  const email = formData.get("email");
  if (!email.includes("@")) {
    return { error: "Invalid email" };
  }
  await new Promise((res) => setTimeout(res, 2000));
  return { success: "User created successfully" };
}
```

Notice:

* First argument = `prevState`

* Second = `formData`

---

## **2️⃣ Client Component**

```bash
"use client";
import { useActionState } from "react";
import { createUser } from "./actions";

export default function SignupForm() {
  const [state, formAction, isPending] = useActionState(createUser, null);

  return (
    <form action={formAction}>
      <input name="email" placeholder="Enter email" />
      <button type="submit" disabled={isPending}>
        {isPending ? "Creating..." : "Create User"}
      </button>
      {state?.error && <p style={{ color: "red" }}>{state.error}</p>}
      {state?.success && <p style={{ color: "green" }}>{state.success}</p>}
    </form>
  );
}

## **Shadow DOM vs Virtual DOM**
- **What is the difference between Shadow DOM and Virtual DOM?**
   - **Virtual DOM**: A React-specific concept (a JS object) used for performance optimization.
   - **Shadow DOM**: A browser technology used for scoping CSS and HTML (e.g., in Web Components).

## **React Strict Mode in Development**
- **Why does React log twice in Development? (Strict Mode)**
   - React 18+ intentionally mounts/unmounts/remounts components in Strict Mode to help you find side-effect bugs and ensure your cleanup functions work.
