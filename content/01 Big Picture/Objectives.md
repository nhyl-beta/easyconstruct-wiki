---
tags: [easyconstruct, big-picture, manuscript]
aliases: [Thesis Objectives]
---
# Objectives (final manuscript)

> [!summary] 🟢 In simple terms
> The thesis promises four things: **(1)** one cloud system for the whole company, **(2)** a workforce tool for attendance, pay and tasks, **(3)** "AI" project intelligence that helps people decide, and **(4)** reports for managers. These four are **fixed** — the system was changed to meet them, not the other way round.

> [!note] Evolution
> The Chapter 1 proposal (early 2026) listed **eight** objectives (separate ones for cloud platform, AI requirements, AI workflow orchestration, AI issue detection, AI impact awareness…). The Capstone 2 manuscript merged them into the **four** below.

## The four objectives → where they live in the code
| # | Objective | Implemented by |
|---|---|---|
| 1 | **Cloud-based unified system** centralizing finance, HR, project management and organizational data | All 9 [[API Modules]]; one PostgreSQL database ([[Database and Drizzle]]); hosted on cloud ([[Deployment and Hosting]]); data consistency checks ([[Reconciliation Checks]]) |
| 2 | **Workforce management** — attendance, gross labor computation, task allocation | [[Attendance and Geofencing]], [[Payroll Engine]], tasks + assignee suggestions, workforce reports |
| 3 | **AI-driven project intelligence** — requirement generation, proposal validation, task/schedule suggestions on approved proposals, issue detection with resolutions, impact forecasting | `insights` module → [[Decision Support Overview]], [[Signals]], [[Proposal Validation and Cost Reference]], [[Plan Suggestion and Impact Forecast]] |
| 4 | **Consolidated operational & analytical reports** | Role dashboards, Owner/Admin cycle times, PM daily/weekly/monthly reports, workforce & fairness reports, exports (considered fully implemented by Nov 2026) |

## How the objectives were tracked
- An **objectives fix list** (Oct 2026) broke each objective into numbered fixes (e.g. *fix 1.3* reconciliation, *fix 3.1* plan suggestion, *fix 3.2* impact forecast, *fix 3.5* signal playbook). You can still see these numbers in code comments, e.g. `// apps/api/src/forecast/change-impact.ts (objectives fix 3.2)`.
- Dropped from scope during that work: reimbursement "My claims", the "My attendance and pay" employee page, ORRA branding, email copies of notifications.

Related: [[Scope and Limitations]] · [[What is EasyConstruct]]
