---
tags: [easyconstruct, defense, questions, technical]
aliases: [Technical Defense Questions, Tech Questions]
---
# Technical Questions

> [!summary] 🟢 In simple terms
> Questions about **how the thing is actually built**. The Programmer usually fields these, but a panel can aim any of them at anyone — so everyone should know the top section at minimum.

## Stack & architecture
> [!question] Explain your system architecture.
> A single-page React app talks over HTTPS/JSON to an Express REST API, which uses Drizzle ORM against PostgreSQL; files go to S3-compatible object storage, email through an SMTP provider, and scheduled jobs are triggered by CI cron hitting protected endpoints. Draw the boxes in that order. → [[Architecture Overview]]

> [!question] Why PERN? Why not PHP/Laravel or MERN?
> One language end to end (TypeScript) means shared types between frontend and backend and one skill set for the team. PostgreSQL over MongoDB because the domain is **highly relational** — projects, members, milestones, tasks, approvals, payroll all reference each other, and we need transactional integrity for approvals and payroll. → [[Tech Stack]]

> [!question] Why a monorepo?
> The web app, the API and the shared types live in one repository with npm workspaces, so a type change in a shared contract breaks the build immediately on both sides instead of silently at runtime. → [[Monorepo Structure]]

> [!question] What design pattern does your backend follow?
> Layered: **routes → controller → service → repository**. Routes declare guards, controllers handle HTTP shape, services hold business rules, repositories hold SQL. Business logic never touches `req`/`res`, which is what makes it unit-testable without a database. → [[Backend Layering]]

> [!question] How is the backend organized?
> Nine feature modules — `iam`, `documents`, `projects`, `design`, `workflow`, `workforce`, `finance`, `insights`, `platform` — each with a manifest, mounted from a registry. → [[API Modules]]

> [!question] Walk me through what happens when a user clicks a button.
> Pick one flow and narrate it end to end: click → hook → service → HTTP request with JWT → route guard checks role → controller validates input → service applies business rules → repository runs SQL → response → cache/state update → UI re-render. → [[Request Journey]]

---

## Database
> [!question] Why PostgreSQL?
> Relational integrity, transactions, mature constraint support, and real SQL for the reporting queries. Also free managed hosting at our scale. → [[Database and Drizzle]]

> [!question] Why Drizzle ORM and not Prisma or raw SQL?
> Drizzle gives TypeScript-typed queries that compile to predictable SQL with no hidden query generation, and its migrations are plain SQL files we can read and review. Raw SQL alone would lose type safety across the shared contracts. → [[Database and Drizzle]]

> [!question] Is your database normalized?
> Yes — be ready to name the normal form you targeted and point at one example (e.g. project membership as its own table rather than repeating role data on the project row). Then note where you deliberately denormalized for read performance, if anywhere. → [[Database and Drizzle]]

> [!question] How do you handle schema changes?
> Versioned SQL migration files committed to the repo and applied in order, so staging and production converge to the same schema. → [[Database and Drizzle]] · [[Environments and Branches]]

> [!question] What happens if two people approve the same thing at the same time?
> Approval state transitions run inside database transactions, and the scheduled overdue sweeper uses a transaction-level lock so a job can't run twice concurrently. The second approver sees the already-decided state rather than overwriting it. → [[Workflows and Approvals]]

---

## Security
> [!question] How does authentication work?
> Credentials are checked against a bcrypt hash; on success the API issues a JWT bearer token the SPA sends on every request. Login is rate-limited per IP (correctly resolved behind the proxy). → [[Authentication and Sessions]]

> [!question] How do you stop a user from seeing data they shouldn't?
> **Three layers.** (1) Role guard on the route — `requireRole(...)` returns 403 for the wrong role. (2) Project visibility — engineers, architects, site personnel and consultants only see projects they're staffed on, and the cache key includes that scope so cached rows never leak across users. (3) Business authority — only *this* project's PM may approve; an Admin override requires a written reason of at least 10 characters and is recorded in the audit trail. → [[Access Control]]

> [!question] The frontend hides buttons based on role — isn't that insecure?
> The frontend hiding is **UX only**; the server is the real lock. Every protected route carries its own guard, and a test reads every route's declared roles and compares them against the expected access matrix without needing a database. → [[Access Control]]

> [!question] What security issues did you find and fix?
> A dedicated security and crash review on 12 November closed 13 findings, and platform hardening included constraining large JSON bodies to upload routes only and fixing the login limiter's IP resolution behind the proxy. → [[Security Hardening Nov 2026]]

> [!question] Are there any routes without login?
> Deliberately few: health check, login and password reset, two token-link endpoints (public RFI replies and owner approvals), and the cron endpoints which require a shared secret. → [[Access Control]]

---

## Performance, reliability, operations
> [!question] Will it handle many users? How does it scale?
> Honest scoping first: it's sized for a single firm, not for thousands of tenants. Then the specifics — cached read paths with scope-aware keys, pagination on list endpoints, and a stateless API that can run multiple instances behind the host's load balancer. → [[Caching and Performance]]

> [!question] How do you keep the UI live without constant polling?
> A realtime live query drives the notification inbox, and the "Waiting on you" card and approvals badge refresh live rather than on a timer. → [[Notifications and Realtime]]

> [!question] What happens if the server goes down? Do you have backups?
> Scheduled database dumps are pushed to object storage by a cron job, and a smoke test runs against the live API every 30 minutes so failures are noticed quickly. → [[Scheduled Jobs and Backups]]

> [!question] Where are uploaded files stored, and why not in the database?
> S3-compatible object storage, because blueprints and photos are large binaries — storing them in Postgres would bloat backups and slow queries. The database keeps metadata and the storage key. Blueprint uploads also generate a page-1 preview image. → [[Documents and Files]]

> [!question] How do you deploy? Do you have CI/CD?
> Static frontend to a CDN host, the API as a container, both from the same repository; CI runs API and web tests plus type checks on every push, with a type-error baseline that can only decrease. → [[Deployment and Hosting]] · [[Environments and Branches]]

> [!question] How do you know who did what?
> An activity log with full filtering by date range, user, project, module and action, plus exact timestamps; override approvals record the stated reason. → [[Security Hardening Nov 2026]]

---

## The decision-support layer (technical angle)
> [!question] What algorithm does your proposal matching use?
> Dice coefficient for text similarity against a construction cost catalog at a threshold of 0.40, and title-similarity ≥ 0.7 combined with an amount within ±10% inside a 30-day window for duplicate detection. → [[Proposal Validation and Cost Reference]]

> [!question] Where do your thresholds come from, and can they be changed?
> They live in a single configuration file (`core/config/signals.ts`) rather than scattered through the code, so they can be tuned without touching logic. Be honest that initial values were chosen from observed data and client discussion, not from a published standard. → [[Signals]]

> [!question] Can you turn the AI features off?
> Yes — a feature flag on both server and client disables the entire layer, and the rest of the system works unchanged. That separation is what proves the decision-support boundary architecturally. → [[Decision Support Overview]]

> [!question] How is payroll computed? How do you know it's correct?
> Gross labor from attendance, then employee shares for SSS, PhilHealth and Pag-IBIG from published schedules; the engine is unit-tested (`engine.test.ts`, `payroll-rules.test.ts`, `trace.test.ts`) and every computation keeps a trace of how it was derived. State the limitation unprompted: no 13th-month, no de minimis, no year-end annualization, and it never remits. → [[Payroll Engine]]

Related: [[Defense Questions Overview]] · [[Programmer Questions]] · [[Architecture Overview]] · [[Access Control]]
