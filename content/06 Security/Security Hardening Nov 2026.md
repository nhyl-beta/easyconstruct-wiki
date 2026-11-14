---
tags: [easyconstruct, security, november-2026]
aliases: [Security Review, 12 Nov Review, Audit Trail]
---
# Security Hardening (Nov 2026)

> [!summary] 🟢 In simple terms
> On **12 Nov 2026** the whole system was reviewed for security holes and crash risks. 13 items were fixed in one commit on `thesis-final-staging`; 5 items need a decision or a hosting setting. This is in addition to the protections built earlier (login limits, security headers, audit logs, file checks).

## Fixed (commit `07db540`)
| # | Problem (plain words) | Fix |
|---|---|---|
| 1 | Uploads silently saved to server disk (wiped on every deploy) when S3 settings were missing | API refuses to start in production without S3/Blob |
| 2 | "Today" used UTC → wrong between 00:00–08:00 Manila | `todayManila()` |
| 3 | A built-in JWT secret was used unless `NODE_ENV` was exactly `production` | Secret required except in dev/test |
| 4 | Every sign-in cleared all caches | Auth writes don't bump caches |
| 5 | Live notifications died after a server restart | Auto-reconnect 5 s → 60 s |
| 6 | A page crash showed a white screen | Error boundary + Reload |
| 8 | `multer` / `body-parser` DoS advisories | `npm audit fix` |
| 10 | Upload errors named wrong size limits | Corrected messages |
| 12 | One anonymous RFI-reply request could hold 500 MB in memory | Token checked before reading files; 5 × 4 MB cap |
| 13 | A changed role kept old powers up to 30 days | Role change signs the account out |
| 14 | Any signed-in user could open a project file if they knew its URL | File access follows project membership |
| 15 | Password-reset emails had no rate limit | Shares the login limit (40/15 min) |
Checks after: API type-check clean, **1,090 API unit tests pass**, web 411/412.

## Still open
| # | Item | Needs |
|---|---|---|
| 7 | GitHub disables scheduled workflows after 60 days without commits | Second trigger (cron-job.org/UptimeRobot) |
| 9 | Payroll rates are constants in code | Rates table with effective dates (~½ day) |
| 11 | API build skips type checking (`--noCheck`) | Type-error baseline → 0 |
| 16 | `MAX_UPLOAD_MB` defaults to 500 | Set ~50 on Render |
| 17 | No Content-Security-Policy | Browser test with maps/fonts/uploads |

## 🔵 Protections that already existed
- Login throttle (5 failures / account / 15 min) + per-address limit; security headers; single CORS origin.
- **Audit trail** (`audit_logs`): sign-ins, workflow decisions, project edits with field diffs, file downloads (Legal = `sensitive_download`), overrides with reasons.
- File magic-byte sniffing + SHA-256 integrity; authenticated file route.
- Demo/reset scripts refuse to run in production (`ALLOW_DEMO_RESET`, host allowlist).
- Secrets only in host env / GitHub secrets; personal notes kept out of the repo.

Related: [[Authentication and Sessions]] · [[Access Control]] · [[Documents and Files]]
