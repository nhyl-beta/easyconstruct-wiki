---
tags: [easyconstruct, defense, questions, role/project-manager]
aliases: [PM Questions, Project Manager Defense Questions]
---
# Project Manager — Defense Questions

> [!summary] 🟢 Who gets these
> The team member who served as **Project Manager of the capstone team**. Expect two flavours: questions about **how you ran the team**, and questions about **the PM module inside the system**. Panels mix them freely, so know both.

## Running the team
> [!question] What was your role in this project?
> Be concrete about decisions you owned: scope control, task assignment, schedule, client coordination, and release readiness. Avoid "I managed the team" with no verbs after it.

> [!question] What development methodology did you follow and why?
> Iterative delivery against a fixed objective set — the four objectives couldn't move, so each iteration took a numbered slice of them. Work was tracked as per-objective fix lists (e.g. *fix 1.3* reconciliation, *fix 3.2* impact forecast), and those numbers are still visible in code comments. Waterfall was rejected because requirements kept sharpening with each client conversation. → [[Objectives]]

> [!question] How did you break down and assign work?
> Explain your real mechanism — fix lists per objective, bug batches during QA (B-1 through B-225 in November), and per-role test cases. The useful detail is that work items were **numbered and traceable to an objective or a bug**, not assigned ad hoc.

> [!question] How did you handle scope creep?
> Two answers, both true: features were **added** where the client's real workflow demanded them (RFI/RFA module, Design project type, procurement, owner email approvals, support requests), and features were **cut** when they didn't serve the four objectives (reimbursement claims, employee self-service pay page, notification email copies). Every change was decided against the objectives, not against enthusiasm. → [[Scope and Limitations]]

> [!question] What was your biggest risk, and how did you mitigate it?
> A strong, honest one: **late defects**, because manual testing by two people across ten roles couldn't cover the matrix in time. Mitigation was building the automated QA suite in the final month — which immediately exposed problems manual testing had missed, including a test runner silently executing only 5 of 111 test files. → [[Testing Strategy]]

> [!question] Did you fall behind schedule? What did you do?
> Don't claim a perfect schedule — no panel believes it. Name a real slip, what you cut or resequenced, and what shipped anyway. November was explicitly a consolidation month: polish, bug batches, a security review, and the move to Chapter 5. → [[November 2026 Updates]]

> [!question] How did you handle a member who wasn't delivering?
> Answer process, not personalities: reassignment of the blocked item, pairing, and shortened check-in cycles. Keep it brief and non-accusatory — the panel is testing your professionalism, not collecting gossip.

> [!question] How did you coordinate with the client?
> Through requirements interviews and review sessions whose outputs are recorded in your interview documents, with decisions logged as numbered role decisions (e.g. the November decisions about what Admin may write, and who may upload documents). → [[November 2026 Updates]]

> [!question] How did you know when a feature was "done"?
> Definition of done was layered: unit tests pass, the role access matrix check passes, the QA case for that role passes, and the manual tester signs off in the per-role sheet. → [[Testing Strategy]]

> [!question] How did you track progress overall?
> Objective-level fix lists for build work, bug batches with IDs for QA work, and commit history on the staging branch — 74 commits on `thesis-final-staging` between 1 and 13 November alone. → [[Development Timeline]]

---

## The PM module inside the system
> [!question] What can a Project Manager do in EasyConstruct?
> Owns the project's delivery: staffs the team, creates and dates milestones, assigns tasks, advances the project through phases, approves within their authority, and sees daily/weekly/monthly project reports. → [[Project Manager]]

> [!question] Can any PM approve any project?
> No. Only **this project's** PM may approve its items; another PM gets a 403 naming the rule. An Admin may override, but must supply a reason of at least 10 characters, and the override is written to the audit trail. → [[Access Control]]

> [!question] How does the system stop a PM from advancing a project that isn't ready?
> Phase gates. Each phase has named checks that must all pass — for example, leaving Proposal requires an architect and consultant staffed, a submitted proposal linked to a workflow, an approved proposal, and a Notice of Award plus contract on file with a contract value above zero. Each check names the role responsible for fixing it, which drives that person's "Waiting on you" card. → [[Lifecycle Gates]]

> [!question] Walk us through one gate in detail.
> Use P1 — *Architect staffed*: the check looks at the project's members, passes if any member has the architect role, and otherwise fails with the detail "No architect staffed on this project," assigned to the PM with a link to the page that fixes it. → [[Lifecycle Gates]]

> [!question] Does the AI decide whether a project can advance?
> No, and it's enforced architecturally — signals are appended to the lifecycle view only and never reach `canAdvance`; a boundary test fails the build if that ever changes. → [[Decision Support Overview]]

> [!question] How does a PM know what needs their attention?
> The "Waiting on you" card, driven by the `ownerRoles` field on every failing gate check, plus a live-refreshing approvals badge and notification inbox. → [[Notifications and Realtime]]

> [!question] What reports does a PM get?
> Project-level daily, weekly and monthly reports, plus progress that counts **both tasks and live milestones** — note that this differs from the manuscript's description of tasks alone, which is a known mismatch. → [[Project Lifecycle]] · [[00 Start Here]]

Related: [[Defense Questions Overview]] · [[Project Manager]] · [[Lifecycle Gates]] · [[General Questions]]
