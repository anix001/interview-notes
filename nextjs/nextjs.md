---
layout: default
title: Next.js
nav_order: 12
---

# ▲ Next.js Interview Questions

Everything you need to crack Next.js interviews — from routing and rendering to caching and deployment.

---

## 📑 Table of Contents
- [1️⃣ What is Next.js & Why Use It?](#1️⃣-what-is-nextjs--why-use-it)
- [2️⃣ App Router vs Pages Router](#2️⃣-app-router-vs-pages-router)
- [3️⃣ Server Components vs Client Components](#3️⃣-server-components-vs-client-components)
- [4️⃣ Rendering Strategies: SSR, SSG, ISR, CSR](#4️⃣-rendering-strategies-ssr-ssg-isr-csr)
- [5️⃣ Data Fetching in App Router](#5️⃣-data-fetching-in-app-router)
- [6️⃣ getServerSideProps vs getStaticProps](#6️⃣-getserversideprops-vs-getstaticprops)
- [7️⃣ Dynamic Routes](#7️⃣-dynamic-routes)
- [8️⃣ API Routes / Route Handlers](#8️⃣-api-routes--route-handlers)
- [9️⃣ Middleware](#9️⃣-middleware)
- [🔟 next/image](#🔟-nextimage)
- [1️⃣1️⃣ next/link](#1️⃣1️⃣-nextlink)
- [1️⃣2️⃣ Loading & Error UI](#1️⃣2️⃣-loading--error-ui)
- [1️⃣3️⃣ Next.js Caching (4 Layers)](#1️⃣3️⃣-nextjs-caching-4-layers)
- [1️⃣4️⃣ Environment Variables](#1️⃣4️⃣-environment-variables)
- [1️⃣5️⃣ next build & Deployment](#1️⃣5️⃣-next-build--deployment)

---

## **1️⃣ What is Next.js & Why Use It?**

Next.js is a **React framework** that adds server-side features React alone doesn't have out of the box.

* **File-based routing:** Create a file in `app/` or `pages/` → it becomes a route automatically. No React Router needed.
* **Built-in rendering modes:** SSR, SSG, ISR, and CSR all in one framework.
* **API routes:** Build backend endpoints in the same project — no separate Express server needed.
* **Performance out of the box:** Automatic code splitting, image optimization, and prefetching.

> [!TIP]
> **Interview Gold:** Next.js solves the two biggest React limitations — SEO (React renders in the browser, so crawlers see an empty page) and performance (SSR/SSG sends pre-rendered HTML instantly). That's why teams choose it.

---

## **2️⃣ App Router vs Pages Router**

Next.js has two routing systems. Interviewers always ask which one you've used.

### **Pages Router (old, `pages/` folder)**
* Been around since Next.js v1. Stable and widely used in production.
* Data fetching via special functions: `getServerSideProps`, `getStaticProps`.
* Every file in `pages/` is a route.

### **App Router (new, `app/` folder — Next.js 13+)**
* Introduced in Next.js 13, stable in Next.js 14.
* Uses React Server Components by default.
* Data fetching via `async/await` directly in components — no special functions needed.
* Supports nested layouts, `loading.tsx`, `error.tsx`, and more.

```
app/
├── layout.tsx        ← root layout (wraps all pages)
├── page.tsx          ← homepage "/"
├── about/
│   └── page.tsx      ← "/about"
└── blog/
    └── [id]/
        └── page.tsx  ← "/blog/123"
```

> [!IMPORTANT]
> **In new projects, always use the App Router.** Pages Router is still supported but won't get new features.

---

## **3️⃣ Server Components vs Client Components**

### **Server Components (default in App Router)**
* Run **only on the server**. Their code never ships to the browser.
* Can directly `await` database calls, API calls — no need for `useEffect`.
* Cannot use browser APIs (`window`, `localStorage`) or React hooks (`useState`, `useEffect`).

```javascript
// app/page.tsx — Server Component by default
async function Page() {
  const data = await fetch("https://api.example.com/posts").then(r => r.json());
  return <ul>{data.map(p => <li key={p.id}>{p.title}</li>)}</ul>;
}
```

### **Client Components (`"use client"` directive)**
* Add `"use client"` at the top of the file to opt in.
* Run in the browser. Can use hooks, event listeners, and browser APIs.

```javascript
"use client";
import { useState } from "react";

export default function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(c => c + 1)}>{count}</button>;
}
```

> [!TIP]
> **Interview Gold:** Keep as much as possible in Server Components (smaller JS bundle, faster page). Push to Client Components only when you need interactivity or browser APIs. Think "server by default, client only when necessary."

---

## **4️⃣ Rendering Strategies: SSR, SSG, ISR, CSR**

### **SSR — Server Side Rendering**
* HTML is generated **on each request** on the server.
* Always fresh data. Slightly slower TTFB (Time To First Byte).
* Use for: dashboards, user-specific pages, anything that changes per request.

### **SSG — Static Site Generation**
* HTML is generated **at build time** and served as a static file.
* Blazing fast. Can be cached on a CDN.
* Use for: blogs, marketing pages, docs — content that rarely changes.

### **ISR — Incremental Static Regeneration**
* Like SSG, but pages can be re-generated **in the background** after a set time.
* Best of both worlds — fast static files + semi-fresh data.
* Use for: e-commerce product pages, news articles.

### **CSR — Client Side Rendering**
* The server sends a blank HTML shell. React runs in the browser and fetches data.
* Same as plain React. Worst for SEO.
* Use for: dashboards behind a login where SEO doesn't matter.

> [!NOTE]
> **Simple analogy:** SSG = cook food in advance. SSR = cook fresh every order. ISR = cook in advance but refresh the menu every hour. CSR = give the customer an empty plate and let them fill it themselves.

---

## **5️⃣ Data Fetching in App Router**

In the App Router, you fetch data by using `async/await` directly in a Server Component. No `useEffect`, no `getServerSideProps`.

### **SSR — Fresh data every request**
```javascript
async function Page() {
  const res = await fetch("https://api.example.com/data", {
    cache: "no-store", // never cache — always fresh
  });
  const data = await res.json();
  return <div>{data.title}</div>;
}
```

### **SSG — Cached forever (build time)**
```javascript
const res = await fetch("https://api.example.com/data"); // cache: 'force-cache' is default
```

### **ISR — Revalidate every N seconds**
```javascript
const res = await fetch("https://api.example.com/data", {
  next: { revalidate: 60 }, // regenerate at most once every 60 seconds
});
```

> [!TIP]
> **Interview Gold:** In the App Router, `fetch()` is extended by Next.js with caching options. The **`cache`** and **`next.revalidate`** options on `fetch()` directly control SSR vs SSG vs ISR behavior — no special wrapper functions needed.

---

## **6️⃣ getServerSideProps vs getStaticProps**

These are **Pages Router** functions. You'll still see them in most existing codebases.

### **`getServerSideProps`** — SSR
* Runs on the **server on every request**.
* Has access to `req`, `res`, cookies, headers.
```javascript
export async function getServerSideProps(context) {
  const data = await fetchData(context.params.id);
  return { props: { data } };
}
```

### **`getStaticProps`** — SSG
* Runs **at build time only**.
* Use `revalidate` for ISR.
```javascript
export async function getStaticProps() {
  const data = await fetchData();
  return { props: { data }, revalidate: 60 }; // ISR: refresh every 60s
}
```

### **`getStaticPaths`** — required with `getStaticProps` for dynamic routes
* Tells Next.js which dynamic paths to pre-build.
```javascript
export async function getStaticPaths() {
  return {
    paths: [{ params: { id: "1" } }, { params: { id: "2" } }],
    fallback: "blocking", // generate missing paths on-demand
  };
}
```

> [!NOTE]
> In the **App Router**, these functions are gone. You just `await fetch()` in your Server Component with the appropriate cache option.

---

## **7️⃣ Dynamic Routes**

Dynamic routes let one file handle many URLs by using bracket syntax.

### **Pages Router**
```
pages/blog/[id].tsx  →  handles /blog/1, /blog/2, /blog/abc
```

### **App Router**
```
app/blog/[id]/page.tsx  →  same thing
```

```javascript
// app/blog/[id]/page.tsx
export default function BlogPost({ params }: { params: { id: string } }) {
  return <h1>Post {params.id}</h1>;
}
```

### **`generateStaticParams`** (App Router equivalent of `getStaticPaths`)
```javascript
export async function generateStaticParams() {
  const posts = await getPosts();
  return posts.map(p => ({ id: String(p.id) }));
}
```

### **Catch-all routes**
```
app/docs/[...slug]/page.tsx  →  handles /docs/a/b/c
```

> [!TIP]
> **Interview Gold:** Use `[...slug]` for catch-all routes (matches any depth) and `[[...slug]]` for optional catch-all (also matches the root `/`).

---

## **8️⃣ API Routes / Route Handlers**

Next.js lets you write backend API endpoints inside the same project.

### **Pages Router** — `pages/api/`
```javascript
// pages/api/hello.js  →  GET /api/hello
export default function handler(req, res) {
  res.status(200).json({ message: "Hello" });
}
```

### **App Router** — Route Handlers (`app/api/route.ts`)
```javascript
// app/api/users/route.ts  →  /api/users
import { NextResponse } from "next/server";

export async function GET() {
  const users = await getUsers();
  return NextResponse.json(users);
}

export async function POST(request: Request) {
  const body = await request.json();
  const user = await createUser(body);
  return NextResponse.json(user, { status: 201 });
}
```

> [!TIP]
> **Interview Gold:** Route Handlers replace `pages/api/`. Name your exports after the HTTP method (`GET`, `POST`, `PUT`, `DELETE`) — Next.js automatically routes the right method to the right function.

---

## **9️⃣ Middleware**

Middleware is code that runs **before a request reaches your page or API route**. It runs at the Edge (close to the user, very fast).

* File location: `middleware.ts` at the project root (next to `app/` or `pages/`).
* Common uses: authentication checks, redirects, A/B testing, setting headers.

```javascript
// middleware.ts
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
  const token = request.cookies.get("token");

  if (!token && request.nextUrl.pathname.startsWith("/dashboard")) {
    return NextResponse.redirect(new URL("/login", request.url));
  }

  return NextResponse.next(); // continue to the page
}

export const config = {
  matcher: ["/dashboard/:path*"], // only run on these routes
};
```

> [!IMPORTANT]
> Middleware runs on the **Edge Runtime** — it has no access to Node.js APIs like `fs`. Keep it lightweight. Don't do heavy DB queries here.

---

## **🔟 next/image**

Next.js's `<Image>` component is an enhanced `<img>` tag that handles optimization automatically.

* **Automatic resizing:** Serves the right image size for each device.
* **Format conversion:** Converts to WebP/AVIF automatically if the browser supports it.
* **Lazy loading:** Images below the fold load only when scrolled into view (built-in).
* **Prevents layout shift:** Requires `width` and `height` props (or `fill` for responsive).

```javascript
import Image from "next/image";

<Image
  src="/hero.png"
  alt="Hero image"
  width={800}
  height={400}
  priority // load eagerly for above-the-fold images
/>
```

> [!TIP]
> **Interview Gold:** Always use `<Image>` from `next/image` instead of a plain `<img>` tag. It automatically optimizes images to save bandwidth and improve Core Web Vitals scores (LCP).

---

## **1️⃣1️⃣ next/link**

`<Link>` from `next/link` enables **client-side navigation** — switching pages without a full browser reload.

```javascript
import Link from "next/link";

<Link href="/about">About Us</Link>
```

### **Key behaviors**
* **Prefetching:** In production, Next.js automatically prefetches the linked page when the link is visible in the viewport. Makes navigation feel instant.
* **Client-side transition:** Only the changed parts of the page update — the layout stays, no full reload.
* **Replaces `<a>` for internal links.** Use a plain `<a>` only for external URLs.

> [!TIP]
> **Interview Gold:** `<Link>` prefetches pages in the background when links are visible. This is why Next.js apps feel fast — the next page is already loaded before the user even clicks.

---

## **1️⃣2️⃣ Loading & Error UI**

App Router has special reserved filenames that automatically handle loading and error states.

### **`loading.tsx`**
* Shows instantly while a Server Component is fetching data.
* Built on React's `<Suspense>` under the hood.

```javascript
// app/dashboard/loading.tsx
export default function Loading() {
  return <p>Loading dashboard...</p>;
}
```

### **`error.tsx`**
* Catches errors thrown anywhere in the route segment.
* Must be a Client Component (`"use client"`).

```javascript
// app/dashboard/error.tsx
"use client";

export default function Error({ error, reset }: { error: Error; reset: () => void }) {
  return (
    <div>
      <p>Something went wrong: {error.message}</p>
      <button onClick={reset}>Try again</button>
    </div>
  );
}
```

### **`not-found.tsx`**
* Renders when `notFound()` is called or a route doesn't exist.

> [!NOTE]
> These files are **co-located** with your route segment. An `error.tsx` in `app/dashboard/` only catches errors in that dashboard section — it won't affect the rest of your app.

---

## **1️⃣3️⃣ Next.js Caching (4 Layers)**

Next.js has 4 built-in caching mechanisms. This trips up many developers.

| Cache | What it stores | Duration |
|-------|---------------|----------|
| **Request Memoization** | Duplicate `fetch()` calls within one render | Single request |
| **Data Cache** | `fetch()` results on the server | Until revalidated |
| **Full Route Cache** | Entire rendered page HTML | Until revalidated |
| **Router Cache** | Client-side page cache in the browser | Session / time-based |

### **Simple explanation**
1. **Request Memoization** — If two Server Components both call the same API URL during one page render, Next.js only makes the HTTP call once. The result is shared automatically.
2. **Data Cache** — `fetch()` results are stored. `cache: 'no-store'` skips it. `revalidate: 60` refreshes it after 60s.
3. **Full Route Cache** — Fully rendered HTML is cached on the server for static routes.
4. **Router Cache** — Pages you've visited are cached in the browser so back-navigation is instant.

> [!TIP]
> **Interview Gold:** Next.js is aggressive with caching by default. If you're seeing stale data, the fix is usually `cache: 'no-store'` on your `fetch()` call, or calling `revalidatePath('/your-route')` after a mutation.

---

## **1️⃣4️⃣ Environment Variables**

Next.js reads `.env.local` (and `.env`, `.env.production`, etc.) automatically.

### **Server-only variables**
```
# .env.local
DATABASE_URL=postgres://...
API_SECRET=abc123
```
These are available in Server Components, API routes, and `getServerSideProps`. **Never exposed to the browser.**

### **Client-side variables — `NEXT_PUBLIC_` prefix**
```
NEXT_PUBLIC_ANALYTICS_ID=UA-12345
```
Any variable prefixed with `NEXT_PUBLIC_` is **bundled into the browser JS** and visible to everyone.

```javascript
// Works in both server and client code
console.log(process.env.NEXT_PUBLIC_ANALYTICS_ID);

// Only works on the server
console.log(process.env.DATABASE_URL);
```

> [!IMPORTANT]
> **Never put secrets in `NEXT_PUBLIC_` variables.** They are baked into the client bundle and visible to anyone who inspects the page source.

---

## **1️⃣5️⃣ next build & Deployment**

### **`next build`**
* Compiles and optimizes your app for production.
* Pre-renders all static pages (SSG).
* Outputs to `.next/` folder.

### **`next start`**
* Runs the production build locally. Requires a Node.js server.
* For static-only exports, use `next export` (or `output: 'export'` in config).

### **Deployment options**

| Platform | Notes |
|----------|-------|
| **Vercel** | Made by the Next.js team. Zero-config, best DX. Recommended. |
| **AWS / GCP / Azure** | Use a Node.js server or Docker container. |
| **Static export** | Set `output: 'export'` in `next.config.js` — outputs pure HTML/CSS/JS. No SSR. |

```javascript
// next.config.js — static export
/** @type {import('next').NextConfig} */
const nextConfig = {
  output: "export",
};
module.exports = nextConfig;
```

> [!TIP]
> **Interview Gold:** Vercel gives each branch a preview URL automatically, making it easy to review changes before merging. It also handles SSR, ISR, Edge Middleware, and image optimization without any server configuration.
