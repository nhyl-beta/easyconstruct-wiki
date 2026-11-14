---
tags: [easyconstruct, architecture, frontend]
aliases: [Frontend, Web App, Refine]
---
# Frontend Architecture

> [!summary] 🟢 In simple terms
> The website is a **single-page app (SPA)**: the browser downloads the app once, then only swaps data in and out as you click — no full page reloads. Each feature (projects, payroll, documents…) lives in its own folder with the same internal layout, so once you understand one, you understand all of them.

## 🔵 Building blocks
| Piece | File(s) | Job |
|---|---|---|
| App shell | `App.tsx` | Refine `<Refine>` with providers, routes, layout, error boundary |
| **Data provider** | `providers/data.ts` | Talks to the API: adds `Authorization: Bearer <token>`, unwraps `{ success, message, data }`, fires a logout event on 401 |
| **Access control provider** | `providers/access-control-provider.ts` + `config/role-resources.ts` | `can({ resource })` → is this page in my role's list? |
| Resources | `providers/resources.ts` | The list of Refine "resources" (pages) |
| Sidebar | `config/sidebar-nav.ts` | Per-role menu; User Support always last |
| Route guard | `config/route-access.ts` | Direct-URL protection (tested in `route-access.test.ts`) |
| Query cache | `lib/query-client.ts`, `lib/query-keys.ts` (`qk`) | TanStack Query cache + consistent keys |
| Live sync | `features/notifications/...` | SSE + Neon Realtime → invalidate queries |
| Role pages | `pages/roles/<role>/` | Dashboards and role-specific screens |

## 🔵 Feature-folder pattern (mirrors the backend)
```
features/projects/
├── repositories/project.repository.ts  ← calls the API (fetch)
├── services/project.service.ts         ← client-side rules/formatting
├── controllers/project.controller.ts   ← composes data for a page
├── hooks/useProjectsPaged.ts           ← React hook pages call
├── components/ProjectsTable.tsx …      ← UI pieces
├── pages/ProjectDetailPage.tsx         ← routed pages
├── models/, types/, constants/, lib/
```
> repository → service → controller → hook → page

## 🔵 Example: a paged, searchable table
`useProjectsPaged()` sends **search (debounced), filters, page, page size** to `GET /api/projects?page=&limit=&search=…`. The server returns one page + `meta.kpis` (headline counts), so the browser **never downloads the full list**. The previous page stays on screen while the next loads.

## 🔵 Role handling on the client
- The logged-in user (`easyconstruct_user`) is kept in session/local storage; `toFrontendRole()` maps `project-manager` → `project_manager`.
- Hidden ≠ secure: every denial is **also** enforced by the API. → [[Access Control]]

## 🔵 UX conventions (Oct–Nov 2026)
Design system on Tailwind (green primary, warm sand secondary, Space Grotesk) · KPI cards are clickable filters · shared filter bar (search, chips, date range) · every paginated table defaults to 10 rows · one status-colour map · responsive layouts (`h-dvh` sidebar on mobile) · error boundary with Reload instead of a white screen.

Related: [[Request Journey]] · [[Tech Stack]]
