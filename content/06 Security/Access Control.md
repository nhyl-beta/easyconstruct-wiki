---
tags: [easyconstruct, security, rbac]
aliases: [RBAC, Role-Based Access Control, Permissions, Authorization]
---
# Access Control (RBAC)

> [!summary] 🟢 In simple terms
> **Authentication** = *who are you?* · **Authorization** = *what may you do?* EasyConstruct answers the second question in **three layers**: your **role** (job), your **project membership** (which projects you're on), and **business rules** (e.g. only *this* project's PM may approve).

## 🔵 Layer 1 — Role guard
```ts
router.post('/', requireRole("project-manager", "admin"), controller.create);
// any other role → 403 "Role 'architect' cannot access this resource"
```
`requireRole` also exposes `requiredRoles` so a test (`role-matrix.test.ts`) can read every route's guard **without a database** and compare it with the expected matrix.

## 🔵 Layer 2 — Project visibility (row-level scope)
`core/access/project-visibility.ts`:
```ts
export const MEMBERSHIP_SCOPED_ROLES = new Set([
  "engineer", "architect", "site-personnel", "consultant",
]);
```
- These roles see **only projects they're staffed on** (lists, documents, designs, files…).
- A PM sees only projects where they're the PM (`pmOwnsProject`).
- Admin and Owner see all; Consultant gets commercial fields stripped.
- The cache key includes this scope, so one user never receives another's cached rows. → [[Caching and Performance]]

## 🔵 Layer 3 — Business authority
`lifecycle/approval-authority.ts → decideAuthority()`:
| Actor | Result |
|---|---|
| This project's PM | ✅ allowed |
| Another PM | ❌ 403 "Only this project's Project Manager can…" |
| Admin with reason ≥ 10 chars | ✅ as **override**, written to audit trail |
| Admin without reason | ❌ 403 `OVERRIDE_REASON_REQUIRED` |
| Anyone else | ❌ 403 |

## 🔵 Public (no-login) routes — deliberately few
`/api/health`, `/api/auth/login` & password reset, `/api/public/rfi-replies` (token link), `/api/public/owner-approvals` (token link), `/api/cron/*` (protected by `CRON_SECRET` bearer).

## 🔵 Frontend mirror
`config/role-resources.ts` lists pages per role; Refine's `accessControlProvider` hides the rest; `route-access.ts` guards direct URLs. Useful for UX — but **the server is the real lock**.

## Matrix testing
The QA spec `01_access_matrix.csv` (pages × 10 roles: Full / View only / Deny) drives automated API checks on the `thesis-final-test` branch. → [[Testing Strategy]]

Related: [[Roles Overview]] · [[Authentication and Sessions]]
