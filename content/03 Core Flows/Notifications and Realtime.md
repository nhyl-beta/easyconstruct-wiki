---
tags: [easyconstruct, flows, realtime, notifications]
aliases: [Notifications, Realtime, SSE, Neon Realtime]
---
# Notifications and Realtime

> [!summary] 🟢 In simple terms
> When something happens that concerns you (a workflow reaches your stage, a worker clocks in outside the site, an RFI is overdue), a **bell notification appears instantly** without refreshing the page. Some things also send an **email** (password reset, owner approval, daily overdue digest, RFI to client).

## 🔵 Three delivery channels
| Channel | Tech | Used for |
|---|---|---|
| **In-app push (v1)** | **SSE** – Server-Sent Events at `GET /api/notifications/stream` | Bell updates |
| **In-app live query (Nov 2026)** | **Neon Realtime** live query on the user's inbox | Bell, "Waiting on you", approvals badge refresh |
| **Email** | SMTP via **Brevo** (port 2525) | Password reset, Owner approval links, overdue daily digest, RFI to client |

### What is SSE?
A **one-way, always-open HTTP connection** from server to browser. The browser's `EventSource` keeps it open; the server writes events down it. Simpler than WebSockets because the browser never needs to talk back on that channel.
- `EventSource` can't send headers, so **this one route** accepts `?token=` (`allowsQueryToken`) — everywhere else a token in a URL is refused (it would leak into logs).
- Since 12 Nov the stream **reconnects automatically** (5 s, doubling to 60 s) after a server restart.

### What is Neon Realtime?
Neon (the Postgres host) can watch a query and **push changes** when rows change. The API "seals" the caller's inbox query (`core/realtime/index.ts`); the web app subscribes with `useLiveQuery` (`NotificationLiveSync.tsx`) and invalidates React Query caches on change.
- Safety check: Realtime is switched **off** if its endpoint ID differs from `DATABASE_URL`'s (otherwise it would watch the wrong database and stay silent).
- If Realtime is off, the API returns 503 and the app simply falls back to SSE/refetch.

## 🔵 Who gets notified
- Phase change → all project members + PM; entering a phase → each role owning one of its gate checks.
- Workflow stage changes; task/issue/design review/requirement/payroll events; RFI create/respond/overdue.
- Outside-geofence clock-in → PM, HR, Admin instantly.
- Project Completed → Owner.
- **Overdue digest:** one email per person per day; Admins get their items + a per-project overview.

Related: [[Scheduled Jobs and Backups]] · [[Caching and Performance]]
