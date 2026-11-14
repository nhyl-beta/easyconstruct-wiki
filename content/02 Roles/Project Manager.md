---
tags: [easyconstruct, roles, role/pm]
aliases: [PM]
---
# Project Manager (PM)
> [!summary] 🟢 Simple
> The **captain of a project**. Creates it, picks the team, sets milestones and tasks, approves materials and RFIs, and pushes the project to the next phase when all checks are green.

## Features (scope)
Dashboard · Project Oversight · Workflow & Milestone Approvals · Document Oversight · Issue Review · Task Management · Report Monitoring (daily/weekly/monthly reports) · Resources & Tools · Requests (RFI/RFA)

## 🔵 What the PM does in each phase
| Phase | PM actions |
|---|---|
| Proposal | Create project, staff Architect + Consultant (P1, P2), approve proposal (last stage), upload Notice of Award + Contract (P5) |
| Design | Staff Engineer, decide design reviews, raise RFIs |
| Pre-Construction | Approve requirements (C1), activate milestones (C3), create tasks, upload Notice to Proceed + set geofence (C5) |
| Construction | Update milestones, resolve issues, sign off budget changes, raise change orders |
| Closeout | Approve Final Inspection, upload Certificate of Completion, PM stage of Closeout workflow |

- **Only the project's own PM** may approve materials/specs and send RFI/RFA (`decideAuthority`). Another PM gets `403`.
- Only PM/Admin can raise **Change Order Requests** (N-5, enforced server-side).
- Gets an instant alert when a worker clocks in **outside the geofence**.
- Receives a **daily overdue digest email**.

Related: [[Project Lifecycle]] · [[Lifecycle Gates]] · [[RFI and RFA]]
