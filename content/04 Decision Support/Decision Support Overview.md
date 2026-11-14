---
tags: [easyconstruct, ai, decision-support]
aliases: [AI, AI Features, Insights, Decision Support]
---
# Decision Support Overview ("the AI")

> [!summary] 🟢 In simple terms
> The manuscript calls these features **AI-driven**. In the code, they are **rule-based and deterministic**: clear formulas and thresholds (e.g. "warn if a cost is 15% above typical"), text-similarity matching, and templates. They **suggest, warn and explain** — they **never approve, reject or change anything**. A person always decides.

> [!important] Why rule-based instead of an LLM?
> 1. **Explainable** — every warning says what it compared against ("median of 5 past Residential proposals").
> 2. **Repeatable** — same input, same output; easy to test and defend in a thesis.
> 3. **Free & offline-safe** — no API key, no cost, no data sent to a model provider.
> 4. Matches the scope limitation: *decision support only* ([[Scope and Limitations]]).

## The feature map
| Manuscript wording | What the code actually does | Note |
|---|---|---|
| AI proposal validation | Compares a proposal with similar past proposals (median ×2 / ×0.4), contract, duplicates (title similarity ≥ 0.7, amount ±10%, 30 days) | [[Proposal Validation and Cost Reference]] |
| AI cost reference | Matches line items to a construction cost catalog (EstimationPro.ai) via Dice similarity ≥ 0.40 | [[Proposal Validation and Cost Reference]] |
| AI requirement generation | Template packs + `structuring.ts`: splits notes into Objectives / Materials / Constraints / Specifications, flags vague phrases | this page ↓ |
| AI workflow orchestration | Draft plan (milestones, tasks, responsibilities) on an approved proposal | [[Plan Suggestion and Impact Forecast]] |
| AI issue detection & resolution | 8 signal rules + "similar issue resolved before" precedents | [[Signals]] |
| AI impact awareness | Forecast of cost, estimate-at-completion and schedule slip before approving a change | [[Plan Suggestion and Impact Forecast]] |
| Data consistency | Read-only reconciliation checks | [[Reconciliation Checks]] |
| Task allocation | Rule-based assignee suggestions + workload warnings | — |

## 🔵 Requirement structuring (Engineer)
`projects/requirements/structuring.ts` — pure function:
1. Split a rough note into clauses.
2. Route each clause to **Objectives / Materials / Constraints / Specifications**.
3. Rewrite a short list of vague phrases — **always reported**, never silent.
4. Suggest commonly required elements the text doesn't mention (from the project-type template pack).

## 🟣 Architecture rules
- All of it lives in the `insights` API module (`ai-validation`, `signals`, `forecast`, `plan-suggestion`, `reconciliation`, `decision-support`) plus small pure helpers in `projects/requirements` and `projects/issues`.
- **Feature flag:** `FEATURE_AI=false` (server) + `VITE_FEATURE_AI=false` (web) turns the whole layer off; the rest of the system works unchanged.
- **Boundary test:** advice can never feed `canAdvance` or gate checks (`lifecycle/boundary.test.ts`).
- **Usefulness tracking:** `decision-support/service.ts` records whether advice was *shown, taken or dismissed* (best-effort, never blocks the user). Good evidence for Chapter 5.
- Thresholds live in one file: `core/config/signals.ts`.

Related: [[Objectives]] · [[Signals]]
