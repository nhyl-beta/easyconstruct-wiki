---
tags: [easyconstruct, architecture, repo]
aliases: [Repository Structure, Folder Structure, Monorepo]
---
# Monorepo Structure

> [!summary] 🟢 In simple terms
> One GitHub repository holds **both apps** (website + API) plus shared code. It's organized like a company building: each department (module) has its own floor, and departments may only talk through the **front desk** (`index.ts`), never by walking into someone else's office.

Repo: `github.com/nhyl-beta/construction-system-easyconstruct` · default branch `thesis-final`

```
.
├── apps/
│   ├── web/                 # React + Refine frontend (Vite)
│   │   └── src/{features, pages, providers, config, components, lib, services, auth}
│   └── api/                 # Express + Drizzle REST API
│       ├── src/main.ts      # starts HTTP server + local scheduler
│       ├── src/app.ts       # middleware chain + module registry
│       ├── src/core/        # plumbing: http, auth, access, audit, storage, db, cache, mail, jobs, realtime
│       ├── src/modules/     # iam, documents, projects, design, workflow, workforce, finance, insights, platform
│       ├── drizzle/         # 47 SQL migrations + snapshots
│       └── scripts/{ops,demo}/
├── packages/
│   ├── shared/              # ROLE_IDS shared by web and api
│   └── config/              # base tsconfig + ESLint
├── infra/{compose,docker}/  # local Postgres 16 + MinIO; Dockerfile
├── docs/{architecture,adr,setup}/
├── .github/workflows/       # ci, cron, backup, smoke
└── scripts/dev.mjs          # runs web + api together
```

## 🔵 The rules (ADR 0001 — "Modular monorepo", 20 Oct 2026)
1. **npm workspaces** for `apps/*` and `packages/*` (`@easyconstruct/web`, `@easyconstruct/api`).
2. `core/` is framework plumbing and **must not import modules**. Dependencies are injected (e.g. job runners passed into the scheduler).
3. A module may import `core/` and its own files. **Cross-module imports only through the other module's `index.ts`** (a "public barrel").
4. Each module has a **manifest** (`module.ts`): route prefixes, middleware, owned tables, feature flag.
5. Schema stays in `core/db/schema/`, migrations in `drizzle/`; a folder move never changes tables.

> [!tip] Why this matters
> Before the refactor, API plumbing and features were mixed at the source root, so it was hard to see who depended on whom. Now a reviewer can check an import path and immediately know whether it's allowed. Lint caught a real violation: `fix(lint): import roleNeedsEmployee through the iam module index` (13 Nov).

Related: [[API Modules]] · [[Backend Layering]] · [[Environments and Branches]]
