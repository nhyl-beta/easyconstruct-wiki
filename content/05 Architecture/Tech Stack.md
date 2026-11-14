---
tags: [easyconstruct, architecture, stack]
aliases: [Technologies, PERN Stack]
---
# Tech Stack

> [!summary] 🟢 In simple terms
> Everything is written in **TypeScript** (JavaScript with types). The screen is built with **React**, the server with **Express on Node.js**, and data is kept in **PostgreSQL**. Extra libraries handle styling, forms, tables, charts, maps and validation.

## Frontend (`apps/web`)
| Tech | What it is | Why EasyConstruct uses it |
|---|---|---|
| **React 19** | UI library of reusable components | Every screen = small components (cards, tables, dialogs) |
| **Refine** | Framework for admin/data-heavy apps | Resources, routing, auth & access-control providers, data provider |
| **React Router 7** | URL → page mapping | `/projects/DEMO-STAGE-3` opens that project |
| **TanStack Query** | Server-data cache in the browser | Fetch once, cache, refetch on change (`qk` query keys) |
| **TanStack Table** | Headless table logic | Sorting, pagination in all lists |
| **Vite** | Dev server + bundler | Fast reload; builds static files for Cloudflare |
| **Tailwind CSS + shadcn/ui (Radix)** | Utility CSS + accessible components | Consistent design system (green primary, sand secondary, Space Grotesk) |
| **react-hook-form + Zod** | Forms + schema validation | Multi-step create wizards |
| **Recharts** | Charts | Dashboards, cash flow |
| **Leaflet** | Maps | Site location & geofence |
| **Neon Realtime client** | Live queries | Live notification inbox |

## Backend (`apps/api`)
| Tech | What it is | Use |
|---|---|---|
| **Node.js** | JavaScript runtime on the server | Non-blocking I/O handles many requests |
| **Express 5** | Minimal web framework | Routers, middleware chain |
| **Drizzle ORM** | Type-safe SQL builder | Schema in TS, migrations in SQL → [[Database and Drizzle]] |
| **PostgreSQL (Neon)** | Relational DB | All data; serverless Postgres with branching |
| **jsonwebtoken (JWT)** | Signed tokens | Stateless login → [[Authentication and Sessions]] |
| **bcryptjs** | Password hashing | 10 salt rounds |
| **Zod** | Runtime validation | Every request body/query validated |
| **Multer** | Multipart uploads | Files |
| **AWS S3 SDK** | Object storage client | Filebase |
| **Nodemailer** | SMTP email | Brevo |
| **Upstash Redis** (optional) | Hosted cache | Falls back to in-memory |

## Tooling
npm workspaces · ESLint · Node test runner · Docker · GitHub Actions · Playwright (QA branch)

## Manuscript software table (for the paper)
Figma 126.1.2 · VS Code 1.138 · React 19 · Node.js · Express 5.2.1 · PostgreSQL 18 · OpenStreetMap API 0.6.

> [!note] Manuscript vs current reality
> The paper lists **Vercel Blob** for file storage; production moved to **Filebase S3** in Oct 2026 (Vercel pricing limits). Vercel adapter remains only for reading old files. Update Chapter 3 if needed. → [[Deployment and Hosting]]

Related: [[Architecture Overview]] · [[Glossary]]
