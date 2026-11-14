---
tags: [easyconstruct, roles, role/admin]
---
# Admin
> [!summary] 🟢 Simple
> The **operations manager of the whole company**. A Project Manager runs *their* projects; the Admin oversees **all** projects, gives final approvals and archives finished projects.

## Features (scope)
Dashboard · Projects (all) · Workflow Oversight · Document Repository · Activity Logs · Security Monitoring · Roles & Permissions (view) · Workflow Configuration (templates, stages) · Approval Hierarchy · User Support

## 🔵 Technical notes
- **Final stage** in most workflow templates (Budget Change, Change Order, Closeout, Design Turnover, Subcontractor onboarding…). → [[Workflows and Approvals]]
- **Override power:** can Advance a blocked phase or act in the PM's place, but only with `override: true` and a reason ≥ 10 characters; it is saved in `project_phase_history` and the audit log (`decideAuthority()` in `lifecycle/approval-authority.ts`).
- **Archive** is Admin-only (`Completed → Archived`). → [[Project Lifecycle]]
- Edits/deletes documents and document types.
- **Read-only** on accounts and employees; no payroll view; sees only *own* support requests.

Related: [[Roles Overview]] · [[Lifecycle Gates]]
