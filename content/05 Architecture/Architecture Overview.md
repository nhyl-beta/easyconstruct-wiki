---
tags: [easyconstruct, architecture, level/simple-to-deep]
aliases: [System Architecture, Architecture]
---
# Architecture Overview

> [!summary] 🟢 In simple terms (restaurant analogy 🍽️)
> - **Frontend (React)** = the **dining room and menu**. What users see and click.
> - **Backend API (Express)** = the **waiter + kitchen**. Takes orders (requests), checks you're allowed to order it, cooks (business rules), returns the dish (JSON).
> - **Database (PostgreSQL on Neon)** = the **pantry**. Where all ingredients (data) are stored in labelled shelves (tables).
> - **Object storage (Filebase S3)** = the **warehouse** for big items (files, photos, blueprints).
> - **Email (Brevo)** & **cron (GitHub Actions)** = the **delivery driver** and the **alarm clock** for daily chores.

## The big diagram
```mermaid
flowchart TB
  subgraph Browser["User's browser / phone"]
    UI["React 19 + Refine SPA<br/>Vite build, Tailwind, shadcn/ui"]
  end
  subgraph CF["Cloudflare Pages"]
    STATIC["Static files<br/>easyconstruct.online"]
  end
  subgraph Render["Render (Docker)"]
    API["Express 5 API<br/>Node.js + TypeScript"]
  end
  DB[("Neon PostgreSQL<br/>+ Neon Realtime")]
  S3[("Filebase S3<br/>files + DB backups")]
  MAIL["Brevo SMTP :2525"]
  GHA["GitHub Actions<br/>CI, cron, backups"]
  REDIS[("Upstash Redis<br/>optional cache")]
  EXT["EstimationPro.ai<br/>cost reference"]

  UI -- loads --> STATIC
  UI -- "HTTPS JSON + JWT" --> API
  UI -- "SSE / live query" --> API
  API -- "Drizzle ORM (SQL)" --> DB
  API -- "S3 API" --> S3
  API --> MAIL
  API --> REDIS
  API --> EXT
  GHA -- "/api/cron/* (secret)" --> API
  GHA -- "pg_dump" --> S3
```

## Architectural style in one table
| Concern | Choice | Note |
|---|---|---|
| App type | **SPA** (single-page app) + **REST API** | [[Request Journey]] |
| Repo layout | **Modular monorepo** (npm workspaces) | [[Monorepo Structure]] |
| Backend pattern | **Layered**: routes → controller → service → repository | [[Backend Layering]] |
| Backend modules | 9 feature modules with a manifest, mounted from a registry | [[API Modules]] |
| Frontend pattern | Feature folders: repository → service → controller → hook → page | [[Frontend Architecture]] |
| Data | PostgreSQL + Drizzle ORM, SQL migrations | [[Database and Drizzle]] |
| Auth | JWT bearer tokens + bcrypt + role guards | [[Authentication and Sessions]] |
| Hosting | Free-tier cloud, no credit card | [[Deployment and Hosting]] |

Why "PERN"? **P**ostgreSQL · **E**xpress · **R**eact · **N**ode — all JavaScript/TypeScript end to end, so one language and shared types across frontend and backend. → [[Tech Stack]]
