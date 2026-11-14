---
tags: [easyconstruct, ai, forecast]
aliases: [Impact Forecast, Plan Suggestion, Impact Awareness, Workflow Orchestration]
---
# Plan Suggestion and Impact Forecast

> [!summary] 🟢 In simple terms
> **Plan suggestion:** once a proposal is approved, the system drafts a starter schedule (milestones + tasks + who's responsible) from a template for that project type. The PM accepts or discards each item.
> **Impact forecast:** before approving a change (budget change, change order), approvers see *"if you approve this, the project will cost ₱X more than the contract and finish about N days later."*

## 🔵 Plan suggestion (`insights/plan-suggestion/`) — objectives fix 3.1
- Input: project type (Commercial, Residential, Infrastructure, Industrial, Renewable Energy) + start date.
- Output: dated milestones, tasks tied to them, responsibilities checklist; **every item states its basis** ("typical duration for a mid-size job").
- **Accepting** an item goes through the normal milestone/task services, so all rules still apply (draft milestones under C3, due dates inside milestone, write locks). Nothing is applied automatically.
- Mounted at `/api/plan-suggestions`; PM and Admin act; Architect, Consultant, Owner read.

## 🔵 Impact forecast (`insights/forecast/change-impact.ts`) — fix 3.2
`GET /api/workflows/:id/impact-forecast?timeImpactDays=N`
| Output | Meaning |
|---|---|
| Cost vs contract | How far the change pushes cost past the contract value |
| Estimate at completion | Projected total cost given spend so far vs work done |
| Schedule slip | Days moved; ⚠ ≥ 7 days, 🔴 ≥ 21 days |

"How much is done" uses **task completion**, not `projects.progress` (progress contains phase-band offsets that would distort the maths).

> [!example] Scenario (from the System Flow paper)
> An architect adds a floor → cost impact ↑ (materials, labor), timeline impact ↑, resource impact (more workers), risk (structural/compliance), recommendation ("review budget allocation before approval").

Related: [[Workflows and Approvals]] · [[Finance Manager]] · [[Decision Support Overview]]
