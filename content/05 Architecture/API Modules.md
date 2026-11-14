---
tags: [easyconstruct, architecture, backend]
aliases: [Modules, Module Registry]
---
# API Modules

> [!summary] 🟢 In simple terms
> The backend is split into **9 departments**. Each one owns certain URLs and certain database tables. A central list (the *registry*) plugs them into the app in a fixed order, and some can be switched off with a flag.

| Module | Owns (examples) | Main URL prefixes | Notes |
|---|---|---|---|
| `iam` | auth, users, roles, user settings, owner approvals, support requests | `/api/auth`, `/api/users`, `/api/roles` | always on |
| `documents` | documents, uploads, document types | `/api/documents`, `/api/uploads` | always on → [[Documents and Files]] |
| `projects` | projects, members, stakeholders, milestones, tasks, issues, lifecycle, revisions, requirements, engineering reports, calendar, dashboard | `/api/projects`, `/api/tasks`, `/api/milestones`… | → [[Project Lifecycle]] |
| `design` | designs, reviews, requests (RFI/RFA), transmittals, deliverables (plan sets), proposals, blueprints, architect documents | `/api/designs`, `/api/proposals`… | → [[RFI and RFA]] |
| `workflow` | workflows, stages, approvals | `/api/workflows` | → [[Workflows and Approvals]] |
| `workforce` | employees, HR, attendance, payroll, workforce reports | `/api/employees`, `/api/attendance`, `/api/payroll` | → [[Payroll Engine]] |
| `finance` | budget, expenses, procurement, cash flow, payroll review… | `/api/finance/*` | → [[Finance Module]] |
| `insights` | ai-validation, signals, forecast, plan suggestion, reconciliation, decision support | various | flag `FEATURE_AI` → [[Decision Support Overview]] |
| `platform` | notifications, audit-log routes, clients/vendors reference, cron routes | `/api/notifications`, `/api/cron` | always on |

## 🔵 How a module plugs in
```ts
// modules/projects/module.ts
export const module: ApiModule = {
  name: "projects",
  flag: "MODULE_PROJECTS",
  mounts: [
    { order: 0,  prefix: "/api/projects",   router: projectRoutes },
    { order: 17, prefix: "/api/tasks",      router: taskRoutes },
    { order: 33, prefix: "/api/milestones", router: milestoneRoutes },
    // ...
  ],
  ownsTables: ["projects", "project_members", "milestones", "tasks", "issues", /* ... */],
};
```
```ts
// app.ts
for (const mount of getEnabledMounts()) {          // filtered by flags, sorted by order
  app.use(mount.prefix, ...(mount.middleware ?? []), mount.router);
}
```
- Flags are **default-on**; setting `MODULE_X=false` turns one off. IAM, documents, platform can't be turned off.
- `order` keeps route matching deterministic (more specific prefixes first).

Related: [[Monorepo Structure]] · [[Backend Layering]]
