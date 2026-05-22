# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repo Is

A static collection of Markdown notes for software engineering interview preparation, hosted on GitHub Pages. There is no build system, no package manager, and no scripts — all content is plain `.md` files.

## Structure

Each technical domain lives in its own directory with a single file:

| Directory | File | Topics |
|-----------|------|--------|
| `ai/` | `ai.md` | MCP, Agentic AI, RAG, LLM fundamentals |
| `aws/` | `aws.md` | EC2, S3, Lambda, cloud fundamentals |
| `db/` | `db.md` | Indexing, ACID, Sharding, NoSQL |
| `design-pattern/` | `design_pattern.md` | MVC, structural patterns |
| `hr/` | `hr.md` | Common HR questions, soft skills |
| `js/` | `js.md` | Closures, hoisting, Promises, ES6+ |
| `node/` | `node.md` | Event loop, streams, worker threads, security |
| `react/` | `react.md` | Virtual DOM, hooks, reconciliation, React 19 |
| `sql/` | `sql.md` | Complex queries, joins, optimization |
| `system-design/` | `system_design.md` | Scalability, load balancing, microservices |

`index.md` is the GitHub Pages landing page. `README.md` is the repo overview.

## File Format

Every topic file uses Jekyll front matter for GitHub Pages navigation:

```markdown
---
layout: default
title: JavaScript
nav_order: 2
---
```

`nav_order` must be unique across all topic files.

## Adding Content

- **New topic**: create a new directory, add a `.md` file with the front matter above (unique `nav_order`), and add a link in `index.md` and `README.md`.
- **Existing topic**: append to the relevant `<domain>/<domain>.md` file.
- Use `[!TIP]`, `[!NOTE]`, and `[!IMPORTANT]` GitHub callout blocks for key "Interview Gold" takeaways.
- Use relative links when cross-referencing topics (e.g., `../js/js.md`).
