---
tags: [easyconstruct, defense, questions, fundamentals, developer]
aliases: [Developer Knowledge, Assumed Knowledge, Fundamentals, Knowledge Check, Basic Questions]
---
# 🧠 Developer Knowledge Check — What You're Assumed to Know

> [!summary] 🟢 Why this note exists
> A panel does not only ask *"how does your system work?"* They ask *"do you understand what you used?"* These are the fundamentals anyone who claims to have built a PERN application is assumed to know. They are often asked casually, mid-answer, as a trap: **"You mentioned JWT — what's actually inside a JWT?"**
>
> Answer each one in **two sentences and one example from your own system**. Long academic definitions sound memorized; a short answer plus "we use it for X" sounds lived.

> [!warning] The failure mode this prevents
> Naming a technology you can't explain is worse than not naming it. If you say "we used Drizzle ORM" and can't say what an ORM *is*, every other claim you make becomes suspect. **Never put a word in your answer you can't define.**

---

## 1. Web and client–server fundamentals
| Question | The answer, short |
|---|---|
| **What happens when a user types your URL and presses Enter?** | DNS resolves the domain to an address, the browser opens an HTTPS connection and requests the page, Cloudflare Pages returns the static React bundle, then the app calls the API for data and renders it. |
| **What is the difference between frontend and backend?** | Frontend runs in the user's browser and shows the interface; backend runs on a server, holds the business rules and talks to the database. **Users can modify anything in the frontend — which is why rules live in the backend.** |
| **Client–server model?** | The client requests, the server responds. They're separate programs on separate machines communicating over HTTP. |
| **What is an API?** | A defined set of endpoints one program uses to ask another for data or actions. Ours is a REST API over HTTPS returning JSON. |
| **HTTP methods you use?** | `GET` read, `POST` create, `PUT`/`PATCH` update, `DELETE` remove. |
| **Status codes you should recognize instantly** | `200` OK · `201` created · `400` bad request · `401` not authenticated · `403` authenticated but **not allowed** (your role guards return this) · `404` not found · `500` server error · `503` unavailable (your email endpoints return this when SMTP isn't configured). |
| **What is JSON?** | A text format for structured data; the common language between the React client and the Express API. |
| **Synchronous vs asynchronous?** | Synchronous waits for each step; asynchronous starts the work and continues. Node is asynchronous, so a slow database query doesn't block other users' requests. |

---

## 2. Databases
| Question | The answer, short |
|---|---|
| **Why a relational database here?** | The data is deeply related — projects have members, milestones, tasks, proposals, approvals — and approvals and payroll need transactions. Relational enforces those relationships; a document store would not. |
| **Primary key vs foreign key?** | A primary key uniquely identifies a row; a foreign key points at another table's primary key. Example: a task's `project_id` is a foreign key to the project. |
| **What is normalization? Which form did you target?** | Organizing tables to remove duplicated data and update anomalies. 1NF: atomic values, no repeating groups. 2NF: no partial dependency on part of a composite key. 3NF: no non-key field depending on another non-key field. **Have one example from your own schema ready** — project membership as its own table rather than role columns repeated on the project. |
| **What is a JOIN?** | Combining rows from two tables on a related column — e.g. attendance joined to employees to compute payroll. INNER returns only matches; LEFT returns all rows from the left table regardless. |
| **What is an index, and what does it cost?** | A lookup structure that makes reads on a column much faster, at the cost of slower writes and extra storage. |
| **What is a transaction? What does ACID mean?** | A group of operations that all succeed or all roll back. Atomicity, Consistency, Isolation, Durability. Used in approvals so a half-finished state can't be saved. |
| **What is an ORM, and why use one?** | A layer that lets you query the database in typed code instead of raw SQL strings. Drizzle gives us TypeScript-checked queries and readable SQL migrations, and parameterizes values — which also prevents SQL injection. |
| **What is a migration?** | A versioned schema change file, applied in order, so every environment reaches the same database structure. |

---

## 3. Authentication and security
| Question | The answer, short |
|---|---|
| **Authentication vs authorization?** | Authentication = *who are you* (login). Authorization = *what may you do* (role and project checks). |
| **What is inside a JWT?** | Three base64 parts separated by dots: **header** (algorithm), **payload** (claims such as user id and role), **signature**. The signature is verified with the server's secret, so the payload can't be tampered with. |
| **Is the JWT payload encrypted?** | **No — it is only encoded and signed.** Anyone can read it. That's why no secret ever goes in a token. This is a favourite trick question. |
| **Why hash passwords instead of encrypting them?** | Encryption is reversible; hashing is one-way, so a database leak doesn't reveal passwords. bcrypt also salts each hash, so identical passwords produce different stored values. |
| **What is SQL injection, and are you protected?** | An attacker supplies input that changes the SQL query. We're protected because the ORM parameterizes values instead of concatenating strings. |
| **What is XSS?** | Injecting script into a page that runs in another user's browser. React escapes rendered values by default. |
| **What is CORS?** | A browser rule controlling which origins may call your API. Ours allows exactly one origin — the public web URL. |
| **Why rate-limit login?** | To slow brute-force password guessing. Ours limits by client IP, resolved correctly behind the proxy. |
| **Principle of least privilege?** | Every user gets the minimum access their job needs — which is the whole reason for ten roles and three layers of access control. → [[Access Control]] |

---

## 4. Frontend
| Question | The answer, short |
|---|---|
| **SPA vs multi-page?** | A single-page app loads once and swaps views in the browser without full page reloads, so navigation feels instant and only data moves over the network. |
| **What is a component?** | A reusable, self-contained piece of UI with its own logic and markup. |
| **State vs props?** | State is data a component owns and can change; props are data passed in from a parent. |
| **What is the virtual DOM?** | React computes the UI in memory, compares it with the previous version, and updates only the parts of the real page that changed. |
| **What is a hook?** | A function that lets a component use React features — `useState` for state, `useEffect` for side effects. |
| **Why TypeScript over plain JavaScript?** | Types catch mistakes at build time rather than in front of a user, and the frontend and backend share the same type definitions, so a contract change breaks the build immediately. |

---

## 5. Backend and architecture
| Question | The answer, short |
|---|---|
| **What is middleware?** | A function that runs between the request arriving and the handler responding — used for authentication, role checks, logging and validation. |
| **What is REST?** | An architectural style where resources have URLs and HTTP methods express the action, with stateless requests. |
| **Why separate controller, service and repository?** | Each layer has one job: controllers handle HTTP, services hold business rules, repositories hold data access. The payoff is that business rules can be unit-tested without a server or database. → [[Backend Layering]] |
| **Why environment variables?** | So secrets and per-environment settings stay out of source control and can differ between local, staging and production. |
| **What is a REST endpoint you built? Describe it.** | Pick one and know its method, path, who may call it, what it validates, and what it returns. |

---

## 6. Software engineering practice
| Question | The answer, short |
|---|---|
| **What is version control / why Git?** | Tracked history of every change, with branches so parallel work doesn't collide and anything can be reverted. |
| **What is a branch? What's your strategy?** | An independent line of work. Ours separates staging integration from the QA automation branch. → [[Environments and Branches]] |
| **What is CI/CD?** | Continuous integration runs tests and type checks automatically on every push; continuous deployment ships what passes. |
| **What is SDLC?** | The phases of building software — requirements, design, development, testing, deployment, maintenance. Be ready to say which model you followed and why. |
| **Unit vs integration vs system testing?** | Unit tests one function in isolation; integration tests modules working together; system testing exercises the whole application. → [[Testing Phases Explained]] |
| **What is regression testing?** | Re-running existing tests to confirm new changes didn't break what already worked. |
| **What is technical debt?** | Shortcuts taken for speed that cost more later. Being able to name a piece of yours is a mark of maturity, not weakness. |

---

## 7. About *this* system — instant recall
> [!important] If you can't answer these, no amount of theory will save the answer
> - **Ten roles:** Owner, Admin, IT Designer, Project Manager, Human Resources, Finance Manager, Architect, Engineer, Site Personnel, Consultant.
> - **Nine API modules:** iam, documents, projects, design, workflow, workforce, finance, insights, platform.
> - **Four layers per module:** schema → repository → service → controller → routes.
> - **Three access layers:** role guard → project visibility → business authority.
> - **Lifecycle phases:** Proposal → Design → Pre-Construction → Construction → Closeout → Completed → Archived, plus the Design-type path ending in Turnover.
> - **Four objectives:** unified cloud system · workforce management · AI-driven project intelligence · consolidated reports.
> - **The stack in one breath:** PostgreSQL, Express, React, Node — TypeScript throughout.

---

## 🚩 Answers that lose marks
> [!warning] Avoid these exact phrases
> - *"I'm not the programmer, so I don't know."* — Say instead: *"[Name] implemented that — may they take it?"* and then **add the one thing you do know about it.** Total ignorance of your own system isn't excused by role division.
> - *"We used it because it's popular / the tutorial used it."* — Every technology needs a reason tied to your requirements.
> - *"It's secure."* — Unsupported. Name a mechanism: hashing, role guards, audit trail.
> - *"It can handle any number of users."* — Nobody believes it and you can't prove it. Say what you sized it for.
> - Naming a technology you can't define. If you're unsure of a term, don't volunteer it.
> - Guessing a number. *"I don't have that figure, but I can tell you we have 1,090 passing API unit tests"* beats an invented statistic that the panel then probes.

> [!tip] The universal recovery line
> *"I don't want to guess at that — let me give you what I can confirm, and we'll verify the rest."* Used once, it reads as integrity. Used four times, it reads as unpreparedness — so use it sparingly and make sure the rest of your answers are solid.

Related: [[Defense Questions Overview]] · [[Technical Questions]] · [[Programmer Questions]] · [[Study Guide by Role]] · [[Glossary]]
