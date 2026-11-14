---
tags: [easyconstruct, ai, data-quality]
aliases: [Reconciliation, Data Consistency]
---
# Reconciliation Checks

> [!summary] 🟢 In simple terms
> An **auditor robot** that re-adds the numbers. It checks that totals the system stored (progress, budget actuals, payroll batch totals) still match the records they came from. It **reports only** — a person decides which side is right.

## Checks (`insights/reconciliation/checks.ts`) — objectives fix 1.3
| Function | Verifies |
|---|---|
| `checkProgress` | Stored project progress = recomputed progress |
| `checkBudgetActual` | Budget "actual" = sum of approved expenses/labor |
| `checkBatchTotals` | Payroll batch total = sum of its lines |
| `checkBatchHours` | Batch hours = verified attendance hours |
| `checkOrphans` | Records pointing at things that no longer exist |
| `checkInactiveMembers` | Inactive employees still staffed on projects |

- Admin panel at `/api/admin/reconciliation` (read-only); lists up to 20 mismatches per check with a link to the first one.
- Supports Objective 1's promise of **data consistency** → [[Objectives]].

Related: [[Decision Support Overview]] · [[Database and Drizzle]]
