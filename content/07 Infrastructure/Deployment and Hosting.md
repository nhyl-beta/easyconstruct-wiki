---
tags: [easyconstruct, infrastructure, deployment]
aliases: [Deployment, Hosting, Production]
---
# Deployment and Hosting

> [!summary] 🟢 In simple terms
> The live system runs on **free cloud services, no credit card**. The website files sit on Cloudflare, the API runs in a Docker container on Render, the database is on Neon, files on Filebase, email through Brevo, and GitHub runs the daily chores. The address is **easyconstruct.online**.

## The production map
| Component | Service | Why |
|---|---|---|
| Web app (static SPA) | **Cloudflare Pages** | Free global CDN, builds from Git |
| API | **Render** (Docker web service, free) | Long-running Node process (SSE needs it) |
| Database | **Neon PostgreSQL** | Serverless Postgres, branching, Realtime |
| Files + DB backups | **Filebase** (S3-compatible, one bucket) | Replaced Vercel Blob |
| Email | **Brevo SMTP, port 2525** | Render blocks ports 25/465/587 |
| Cron & backups | **GitHub Actions** | Free scheduled jobs |
| Keep-awake & monitoring | **UptimeRobot** | Render free tier sleeps when idle |
| Domain & DNS | GoDaddy registrar → **Cloudflare** nameservers | |
| Optional cache | **Upstash Redis** | Falls back to in-memory |

## 🔵 Why it moved off Vercel (Oct 2026)
Vercel's free plan limits (serverless functions, Blob storage, cron frequency) didn't fit a system with live notifications and many uploads. The code still contains `vercel.ts` (serverless export) and a Blob adapter for reading old files, but production is Render + Filebase.

## 🔵 Deploy checklist (from README & 12 Nov review)
1. Set env on Render: `DATABASE_URL` (pooled), `JWT_SECRET`, all four `S3_*`, `CORS_ORIGIN=https://easyconstruct.online`, `APP_URL`, `MAIL_*`, `CRON_SECRET`, `TRUST_PROXY=1`, `DISABLE_SCHEDULER=true`, `AWS_*_CHECKSUM_*=WHEN_REQUIRED` (Filebase).
2. Run migrations **separately** with Neon's **direct** URL (`npm run db:migrate`). The API never migrates on startup.
3. Cloudflare build env: `VITE_API_URL` = API origin + `/api`.
4. After deploy: sign in; PM opens own vs other project's document (second must be refused); role change signs out; RFI reply with attachment; restart API and confirm notifications resume.

## 🔵 Docker
`infra/docker/` holds the API Dockerfile with a **HEALTHCHECK** hitting `/api/health` (no DB call → fast liveness). `/api/health/ready` runs `SELECT 1` (readiness). Graceful shutdown closes the server and pool.

## 🔵 Local development
```bash
npm ci
docker compose -f infra/compose/docker-compose.yml up -d   # Postgres 16 + MinIO
cp apps/api/.env.example apps/api/.env && cp apps/web/.env.example apps/web/.env
npm run db:migrate --workspace=@easyconstruct/api
ALLOW_DEMO_RESET=true npm run db:seed      # one demo account per role
npm run dev                                 # web :5173 + api :8000
```

> [!note] Chapter 5 reality
> As of Nov 2026, EasyConstruct is deployed for testing but **not yet implemented in ORRA's operational environment**, so no production-performance or post-implementation survey results are reported.

Related: [[Scheduled Jobs and Backups]] · [[Environments and Branches]] · [[Architecture Overview]]
