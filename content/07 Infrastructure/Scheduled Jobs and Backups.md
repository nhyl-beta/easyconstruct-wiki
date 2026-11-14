---
tags: [easyconstruct, infrastructure, jobs]
aliases: [Cron, Scheduled Jobs, Backups]
---
# Scheduled Jobs and Backups

> [!summary] 🟢 In simple terms
> Some work must happen **on a timer**, not when someone clicks: scan projects for warnings, check who was absent, send overdue reminders, back up the database. GitHub Actions acts as the **alarm clock** that pokes the API at set times.

## The schedule (`.github/workflows/`)
| Workflow | When (UTC → Manila) | Does |
|---|---|---|
| `cron.yml` → signals | 22:00 → 06:00 | Run all [[Signals]] rules on active projects |
| `cron.yml` → attendance-exceptions | 11:00 → 19:00 | Late / absent detection for HR |
| `cron.yml` → overdue | 01:00 → 09:00 | Overdue RFI/RFA & workflow stages + **one digest email per person** |
| `cron.yml` → design-requests-sweep | every hour at :30 | RFI/RFA overdue sweep |
| `backup.yml` | Sundays 20:00 → Mon 04:00 | `pg_dump` → check dump → upload to Filebase → prune old |
| `smoke.yml` | every 30 min | Live smoke test: API + DB answer, sign-in works, first pages load (read-only) |
| `ci.yml` | every push/PR | Tests, type checks, type-error baseline |

## 🔵 How a cron call is protected
```
GitHub Actions  ──POST /api/cron/signals──▶  API
                Authorization: Bearer $CRON_SECRET
```
`CRON_SECRET` must match on both sides; otherwise 401. `DISABLE_SCHEDULER=true` turns off the in-process timers on Render so jobs don't run twice.

## 🔵 Local mode (`core/jobs/scheduler.ts`)
In development, `setInterval` timers run each job every 6 h (first tick after 60 s). The overdue job uses a **Postgres advisory lock** (`withJobLock`) so two instances can't send duplicate emails; since 10 Nov it's a **transaction-level** lock (safe with pooled connections).

## ⚠️ Gotcha
GitHub **disables scheduled workflows after 60 days without repository activity**. Open item: add a second trigger (cron-job.org / UptimeRobot). → [[Security Hardening Nov 2026]]

Related: [[Notifications and Realtime]] · [[Deployment and Hosting]]
