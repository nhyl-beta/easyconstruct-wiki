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
> [!question] Your tech stack lists Redis — what is it actually used for, and what happens if it's unavailable?
> It's a shared cache for frequently-requested read data, such as dashboard summaries, so repeated requests don't hit the database every time — a serverless-hosted Redis, consistent with the free-tier-first infrastructure choice. It degrades safely: when Redis isn't configured or isn't reachable, the system falls back to an in-memory cache automatically, so a slow or unavailable cache never breaks a request — it just means a cache miss, not an error. → [[Caching and Performance]]

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

> [!question] Does plan suggestion or impact forecast automatically change anything?
> No. Plan suggestion drafts milestones, tasks and a responsibilities checklist from a project-type template once a proposal is approved — the PM accepts or discards each item individually, and an accepted item goes through the same milestone/task services as a manually created one, so every existing rule (draft-only under gate C3, due dates inside the milestone, write locks) still applies. Impact forecast only *computes and displays* projected cost-vs-contract, estimate-at-completion and schedule slip before an approver acts — it never applies the change itself. → [[Plan Suggestion and Impact Forecast]]

> [!question] Your impact forecast measures "how much is done" — how, exactly?
> By task completion, deliberately not the `projects.progress` field, because that field contains phase-band offsets (e.g. a phase is defined as "40–60% of the project") that would distort a cost-per-percent-complete calculation. Using the raw task-completion ratio keeps the math honest. → [[Plan Suggestion and Impact Forecast]]

> [!question] What does the reconciliation checker actually verify, and who fixes a mismatch?
> Six specific checks: stored vs recomputed project progress, budget "actual" vs the sum of approved expenses/labor, payroll batch totals vs their line items, batch hours vs verified attendance, orphaned references, and inactive employees still staffed on a project. It's read-only — it lists mismatches in an admin panel, up to 20 per check with a link to the first one — and a person decides which side is correct and fixes it. It never auto-corrects data. → [[Reconciliation Checks]]

> [!question] How do you know the decision-support layer is actually useful, and not just present for the sake of the "AI" claim?
> `decision-support/service.ts` records, best-effort, whether a piece of advice was **shown, taken, or dismissed** — without ever blocking the user if that recording fails. That's the evidence base for Chapter 5: if a given signal or validation is dismissed far more often than acted on, that's a measurable signal the threshold or the rule itself needs tuning, not a reason to assume it's working. → [[Decision Support Overview]]

> [!question] What happens when a signal's underlying condition goes away — does it keep warning forever?
> No — each finding is stored once per occurrence and **auto-resolves** when the condition clears on a later scan, so there's no notification spam and no stale warning sitting on a project that's already been fixed. → [[Signals]]

> [!question] Could a signal or validation flag be wrong — a false positive?
> Yes, and that's expected of any threshold-based rule — a cost-variance warning at 15% is a judgment call, not a certainty. Two things limit the damage: thresholds live in one config file so they can be retuned without touching logic, and usefulness tracking (shown/taken/dismissed) would surface a rule that's wrong often enough to be dismissed habitually. Nothing a signal says can gate or block anything, so a false positive costs a reviewer's attention, never a stuck workflow. → [[Signals]] · [[Decision Support Overview]]

> [!question] Issue precedent draws on "all projects" — doesn't that leak one project's data to another?
> No — it only ever surfaces an issue's **title, category, resolution note and date**. It deliberately never shows another project's commercial data (cost, budget, contract terms), which is what keeps cross-project learning useful without crossing a confidentiality line between clients or project teams. → [[Signals]]

---

## Cloud-based architecture — going deeper
> [!question] What does "cloud-based" concretely mean for this system, beyond "it's on the internet"?
> No installation on any machine, one database as the single source of truth instead of per-device files, and access from any device with a browser — which is what lets a PM review proposals from the office while Site Personnel log attendance from the field on the same live data. That's the literal mechanism behind Objective 1. → [[Deployment and Hosting]]

> [!question] How do you avoid serving stale cached data right after someone makes a change?
> A version-counter per cache domain, not per key. A write increments the counter for every domain it affects *before* the response is sent; old cache keys are simply never looked up again — nothing has to be found and deleted. Writes that might move progress or status invalidate broadly; narrow writes invalidate only their own domain. → [[Caching and Performance]]

> [!question] Render's free tier sleeps when idle — does a user ever hit a cold start?
> In practice, no: UptimeRobot pings the API every 5 minutes, inside Render's 15-minute idle-sleep window, so the instance stays warm. The health check it hits (`/api/health`) deliberately makes no database call, so it stays fast even under load — a separate `/api/health/ready` endpoint runs an actual `SELECT 1` for true readiness checks. → [[Deployment and Hosting]]

> [!question] How do you actually know the live system is healthy, not just "deployed"?
> A GitHub Actions smoke test runs every 30 minutes against the live site: confirms the API and database answer, sign-in works, and first pages load. That's read-only and separate from the keep-awake ping — one proves the system is reachable, the other proves it's correct. → [[Scheduled Jobs and Backups]]

> [!question] What stops a scheduled job from running twice and sending duplicate emails or notifications?
> Two layers: `DISABLE_SCHEDULER=true` turns off Render's in-process timers in production so only GitHub Actions' cron calls run jobs — no second clock — and the overdue job additionally takes a transaction-level Postgres advisory lock, so even a second concurrent trigger can't double-send. → [[Scheduled Jobs and Backups]]

> [!question] GitHub Actions is free, but is it reliable for production cron?
> With one caveat, stated plainly rather than hidden: GitHub disables scheduled workflows on a public repo after 60 days with no repository activity. That's a known, named open item — the mitigation is keeping the repo active and, longer-term, adding a second trigger such as an external pinger. It's a real constraint of the free-tier choice, not something we're unaware of. → [[Scheduled Jobs and Backups]]

> [!question] If the Render or Neon free tier breaks like Vercel's did, how portable is the system?
> Nothing is hard-locked to one vendor. The API is a Docker container, so it can run on any container host; file storage uses an S3-compatible adapter, so any S3-compatible provider works, not just Filebase; database migrations are plain SQL files that run against any Postgres; and email goes through standard SMTP. The Vercel migration itself is the proof — it happened once already, without a rewrite. → [[Deployment and Hosting]]

Related: [[Defense Questions Overview]] · [[Programmer Questions]] · [[Architecture Overview]] · [[Access Control]]
