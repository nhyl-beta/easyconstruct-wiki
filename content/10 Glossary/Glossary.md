---
tags: [easyconstruct, glossary, level/simple]
aliases: [Terms, Dictionary]
---
# Glossary — tech terms in plain words

> [!tip] How to use
> Each term has a **plain meaning**, an **EasyConstruct example**, and where to read more.

## Web & API basics
| Term | Plain meaning | In EasyConstruct |
|---|---|---|
| **Frontend** | The part you see in the browser | `apps/web` (React) → [[Frontend Architecture]] |
| **Backend** | The server that does the work and stores data | `apps/api` (Express) → [[Backend Layering]] |
| **API** | A menu of requests the backend accepts | `GET /api/projects` |
| **REST** | A style of API: URLs are *things*, HTTP verbs are *actions* | `/api/tasks/42` + `PATCH` |
| **Endpoint / route** | One specific URL + method | `POST /api/projects/:id/lifecycle/advance` |
| **HTTP methods** | GET read · POST create · PATCH/PUT update · DELETE remove | |
| **CRUD** | **C**reate, **R**ead, **U**pdate, **D**elete — the 4 basic data actions | User account management is CRUD on `users` |
| **JSON** | Text format for data: `{ "key": "value" }` | Every reply: `{ success, message, data }` |
| **params** | Values inside the URL path | `/api/projects/:id` → `req.params.id = "12"` |
| **query string** | Values after `?` in the URL | `?page=2&search=bridge` → `req.query` |
| **request body** | Data sent with POST/PATCH | New project form fields |
| **Status code** | Number saying how it went | 200 OK, 401 not logged in, 403 not allowed, 409 blocked by a rule → [[Request Journey]] |
| **SPA** | Single-page app: loads once, swaps data without reloading | The whole web app |

## JavaScript / TypeScript
| Term | Plain meaning | Example |
|---|---|---|
| **TypeScript** | JavaScript + types, so mistakes are caught before running | `progress: number` |
| **Promise** | "I'll give you the result later" | A database query |
| **async / await** | Write "wait for the result" code that reads top-to-bottom without freezing the server | `const user = await repo.findById(id);` |
| **Function (pure)** | Same input → same output, touches nothing else | `computeLine()`, gate checks |
| **Component** | A reusable UI piece | `<ProjectsTable />` |
| **Hook** | A React function that gives a component data/behaviour | `useProjectsPaged()` |
| **State** | Data a component remembers while on screen | Current filters |

## Server concepts
| Term | Plain meaning | Example |
|---|---|---|
| **Middleware** | A checkpoint every request passes through | `authenticate`, `requireRole`, `validate` |
| **Controller / Service / Repository** | Receptionist / expert / librarian layers | → [[Backend Layering]] |
| **Validation (Zod)** | Checking input has the right shape | Due date must be `YYYY-MM-DD` |
| **Environment variable** | A setting outside the code | `DATABASE_URL`, `JWT_SECRET` |
| **Feature flag** | On/off switch for a feature | `FEATURE_AI=false` |
| **Cron job** | A task that runs on a timer | Daily signals scan → [[Scheduled Jobs and Backups]] |
| **Cache / TTL** | Short-term memory / how long it's kept | Projects list 60 s → [[Caching and Performance]] |
| **Pagination** | Showing data page by page | 10 rows per page |

## Data
| Term | Plain meaning | Example |
|---|---|---|
| **Database / table / row / column** | Organized storage like strict spreadsheets | `projects` table |
| **Primary key / foreign key** | Unique ID / a link to another table's ID | `projects.client_id → clients.id` |
| **ORM (Drizzle)** | Write DB queries in TypeScript instead of raw SQL | `db.select().from(projects)` |
| **Schema** | The definition of tables and columns | `core/db/schema/*.ts` |
| **Migration** | A numbered script that changes the DB structure | `0045_blueprint_thumbnail.sql` → [[Database and Drizzle]] |
| **Neon branch** | A copy of the database for testing | Staging experiments |

## Security
| Term | Plain meaning | Example |
|---|---|---|
| **Authentication / Authorization** | Who are you? / What may you do? | → [[Authentication and Sessions]], [[Access Control]] |
| **JWT** | A signed digital pass the browser shows on each request | 30-day token with user id & role |
| **Hash (bcrypt, SHA-256)** | One-way fingerprint; can't be reversed | Passwords (bcrypt), file integrity (SHA-256) |
| **RBAC** | Role-based access control | 10 roles |
| **CORS** | Browser rule: which websites may call the API | Only `easyconstruct.online` |
| **Rate limit / throttle** | Max attempts in a time window | 5 failed logins / 15 min |
| **Audit log** | Permanent record of who did what | Overrides with reasons |

## Realtime & infrastructure
| Term | Plain meaning | Example |
|---|---|---|
| **SSE** | Server keeps a line open and pushes events | Notification bell → [[Notifications and Realtime]] |
| **Live query** | DB pushes changes of a query result | Neon Realtime inbox |
| **Docker / container** | A packaged app that runs the same anywhere | API on Render |
| **CDN** | Copies of static files near users worldwide | Cloudflare Pages |
| **S3 / object storage** | A big bucket for files | Filebase |
| **CI** | Robot that tests every push | `ci.yml` |
| **Monorepo / workspace** | Many apps in one repo | `apps/web`, `apps/api` → [[Monorepo Structure]] |

## Domain & algorithms
| Term | Plain meaning | Example |
|---|---|---|
| **Gate** | Checklist to leave a phase | P1–P5 → [[Lifecycle Gates]] |
| **Workflow / stage** | Multi-signature approval / one signer's step | Budget Change Request → [[Workflows and Approvals]] |
| **RFI / RFA** | Request for Information / Approval | → [[RFI and RFA]] |
| **Geofence / haversine** | Invisible circle / formula for distance on Earth | 300 m default → [[Attendance and Geofencing]] |
| **Dice coefficient** | Text similarity: shared words ÷ total words (×2) | Cost catalog matching → [[Proposal Validation and Cost Reference]] |
| **Median** | Middle value of a sorted list (resists outliers) | Proposal amount checks, payroll rate checks |
| **MSC** | SSS Monthly Salary Credit (₱500 bands) | → [[Payroll Engine]] |
| **NOA / NTP / COC** | Notice of Award / Notice to Proceed / Certificate of Completion | Gate documents |
