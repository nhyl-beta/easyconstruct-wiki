---
tags: [easyconstruct, roles, role/owner]
---
# Owner
> [!summary] 🟢 Simple
> The company boss's view. **Sees everything, changes almost nothing.** Think of it as the CCTV control room.

## Features (scope)
- **Executive dashboard** – project performance, workforce allocation, payroll activity, calendar, AI impact reports, cycle times
- **Portfolio** – every project: status, progress, workforce, budget, due dates, risk
- **Proposals** & **Reports** monitoring (progress / inspection + approval status)
- **Audit Trail** – who did what, when ([[Security Hardening Nov 2026]])
- **System Oversight** – rejected workflows, deleted records, active sessions, failed sign-ins
- **Fail-safe account recovery** – if the IT Designer account is lost/compromised, the Owner recovers it by email

## 🔵 Technical notes
- Sidebar resources: `owner-portfolio`, `owner-proposals`, `owner-audit-trail`, `owner-oversight`, `owner-account-recovery`, `reports`, `support`.
- **Single master Owner account** is enforced.
- **Owner email approval:** before the IT Designer creates an account or touches the Owner account, the Owner approves/declines from an emailed link without signing in (`iam/owner-approvals`, table `owner_action_requests`).
- Notified when a project reaches **Completed**.

Related: [[Roles Overview]] · [[Admin]] · [[IT Designer]]
