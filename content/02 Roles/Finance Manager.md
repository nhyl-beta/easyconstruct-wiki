---
tags: [easyconstruct, roles, role/finance]
aliases: [Finance]
---
# Finance Manager
> [!summary] 🟢 Simple
> The **money gatekeeper**. Sets project budgets, approves or returns payroll from HR, approves expenses and purchases, and checks the cost impact of changes.

## Features (scope)
Dashboard · Approvals (budget change requests, change orders, closeout) · Budget Management (allocations, adjustments, FY filter) · Payroll Review · Expenses (purchase requests, procurement, vendor payments, anomaly flags) · Impact Awareness Review · Reports (cash flow, project profitability)

## 🔵 Technical notes
- **Budget gate C2:** budget lines exist, all approved, total planned > 0. → [[Lifecycle Gates]]
- Final approval of a *Budget Change Request* writes a `budget_adjustments` row and updates `budgets.planned` (G4).
- Approving payroll adds to the project's **Labor** budget and refreshes progress.
- Budget % and "Over budget" include **commitments** (B-43..45 fix).
- Sees the **impact forecast** before approving a change. → [[Plan Suggestion and Impact Forecast]]

Related: [[Finance Module]] · [[Payroll Engine]]
