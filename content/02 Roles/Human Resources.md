---
tags: [easyconstruct, roles, role/hr]
aliases: [HR]
---
# Human Resources (HR)
> [!summary] 🟢 Simple
> Keeps the **list of employees**, checks that the attendance records are real, and prepares the **payroll** for Finance to approve.

## Features (scope)
Dashboard (weekly attendance, department allocation, payroll status) · Employee Record Management · Attendance Monitoring (verify, exceptions, mark absent) · Employee Payroll (tracksheet, gross labor) · Workforce Reports · Approvals (e.g. Subcontractor onboarding — first stage)

## 🔵 Technical notes
- **Only HR creates employees**; IT Designer then links an account.
- Attendance **exceptions**: late after shift start + 15-min grace, likely absences after 12:00 Manila, one-click *Mark absent* — all configurable via env (`ATTENDANCE_*`). → [[Attendance and Geofencing]]
- Payroll batch lifecycle: `draft → pending → approved` or `revision_required` (sent back by Finance). Pre-submit **warnings** (too many hours/day, overtime share > 50%, rate far from position median) must be acknowledged but never block. → [[Payroll Engine]]
- Fairness report: hours, overtime, tasks and pay vs position median.
- Saved filters per user on HR pages.

Related: [[Finance Manager]] · [[Site Personnel]]
