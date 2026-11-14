---
tags: [easyconstruct, infrastructure, performance]
aliases: [Cache, Performance, Pagination]
---
# Caching and Performance

> [!summary] 🟢 In simple terms
> A **cache** is a short-term memory: if 20 people open the dashboard in the same minute, the server computes it once and reuses the answer. When someone **changes** data, the related memories are thrown away so nobody sees stale numbers.

## 🔵 Cache-aside with version counters (`core/cache/`)
```
key:     ec:v1:<domain>:<scope>:<hash(version + query)>
version: ec:v1:<domain>:ver   ← a counter
```
- **Read:** get the domain's version → build key → hit? return : run loader, store with TTL.
- **Write:** the `cacheInvalidation` middleware **increments the counter** of affected domains *before* the response is sent. Old keys are never asked for again — nothing has to be scanned or deleted.
- Writes that might move progress/status (tasks, issues, payroll, lifecycle…) invalidate **every** domain; narrow writes only their own.
- **Scope** in the key (role / user / visibility) prevents one user getting another's rows.
- Store: **Upstash Redis** if configured, else in-memory. The cache **can never fail a request** — errors fall through to the database.

| Endpoint | TTL |
|---|---|
| Dashboard summary | 45 s |
| Projects, proposals, workforce reports | 60 s |
| Workforce board | 30 s |
| Attendance heatmap | 15 s |
| Approval stats | 20 s |
| Workflow templates, roles | 300 s |

## 🔵 Other speed-ups
- **Server-side pagination** everywhere (`page`, `limit`, sort whitelist) — default 10 rows; KPI counts returned in `meta`.
- **Database indexes** on hot filters (documented in `PERFORMANCE.md`).
- **Dashboard summaries** computed in one query instead of many.
- **TanStack Query** on the client: cached, deduplicated fetches; targeted invalidation by query key.
- Small DB pool (5) suited to Neon pooling; 2 MB JSON limit except uploads.

Related: [[Notifications and Realtime]] · [[Frontend Architecture]]
