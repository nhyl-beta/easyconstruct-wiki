---
tags: [easyconstruct, defense, questions, role/system-analyst]
aliases: [SA Questions, System Analyst Defense Questions, Analyst Questions]
---
# System Analyst — Defense Questions

> [!summary] 🟢 Who gets these
> The member who gathered requirements and turned them into the system's design. Panels test whether the design came from **the client's real process** or from your imagination — so every answer should trace back to something ORRA actually said or does.

## Requirements gathering
> [!question] How did you gather your requirements?
> Interviews with ORRA personnel, recorded and transcribed (your `Interview.pdf` and `Interview_Transcript.pdf`), plus observation of their existing paper, spreadsheet and chat-based workflow. Name the methods and say which produced which requirement. → [[The Problem at ORRA]]

> [!question] Who did you interview, and why them?
> Answer from your own records, and justify the selection by **coverage of the workflow** — someone from oversight, someone from delivery, and someone from the back office — so the role modules weren't designed from a single viewpoint.

> [!question] How did you translate what they said into features?
> Walk one thread end to end: a stated pain ("we don't know if a project is really ready to move forward") became a design decision (phase gates), which became specific checks with named owners (contract on file, budget approved, crew staffed), which became code with a test. → [[Lifecycle Gates]]

> [!question] How did you validate that your requirements were correct?
> Review sessions with the client, and decisions recorded and numbered as they were settled — for example the November role decisions on what Admin may write, who may upload documents, and that Owner cannot create documents. → [[November 2026 Updates]]

> [!question] What happened when requirements conflicted?
> Resolve against the objectives: the four objectives were fixed, so a request that didn't serve one was logged as out of scope rather than built. That's how reimbursement claims, the employee self-service pay page and notification email copies were cut. → [[Objectives]] · [[Scope and Limitations]]

---

## Modeling & design decisions
> [!question] Why ten roles? Where did they come from?
> They map to ORRA's actual positions, grouped by function: oversight (Owner, Admin, IT Designer), delivery (Project Manager, Engineer, Site Personnel), design (Architect, Consultant) and back office (Human Resources, Finance Manager). Each has its own module with login and dashboard. → [[Roles Overview]]

> [!question] Your Chapter 1 had a "Super Admin." What happened to it?
> It was split, because analysis showed one account was carrying two genuinely different jobs: **Owner** (business oversight and approvals) and **IT Designer** (system administration and support). Admin became an operational role. Splitting it also improved accountability in the audit trail. → [[Scope and Limitations]]

> [!question] What diagrams did you produce?
> Name what's in your manuscript — context diagram, data flow diagrams, entity-relationship diagram, use case diagrams — and be able to **redraw the ERD's core entities** on request: projects, members, proposals, designs, milestones, tasks, workflows, attendance, payroll. → [[Database and Drizzle]]

> [!question] Explain your as-is versus to-be process.
> As-is: information scattered across paper, spreadsheets and chat; approvals verbal or by message with no trail; attendance and payroll re-typed by hand. To-be: one system where each artifact has a place, each approval has an authority rule and an audit record, and phase advancement requires evidence. → [[The Problem at ORRA]]

> [!question] How did you model the approval process?
> As workflows with stages, where authority is explicit rather than implied: only this project's PM may approve its items, an Admin override requires a written reason of at least 10 characters, and the override is recorded. → [[Workflows and Approvals]] · [[Access Control]]

> [!question] Where are your business rules documented?
> They're not only in the paper — they're expressed as the gate checks themselves, each with a key (P1, C4, X5…), a label, the roles responsible for fixing it, and the condition that makes it pass. The specification and the implementation are the same artifact, which is why they can't drift apart. → [[Lifecycle Gates]]

> [!question] Why gates rather than letting the PM simply mark a phase complete?
> Because the client's actual failures came from phases advancing on assumption — work starting before the contract was on file, or before the budget was approved. A gate makes the precondition **verifiable** instead of asserted. → [[Project Lifecycle]]

---

## Feasibility, data, and boundaries
> [!question] Did you conduct a feasibility study?
> Cover the three standard dimensions with real specifics: technical (the team's existing stack, free-tier infrastructure), operational (role structure mirrors existing positions so retraining is minimal), economic (no licensing cost, no credit card required to run). → [[Deployment and Hosting]]

> [!question] How did you decide what the system should NOT do?
> From the decision-support boundary outward: the system advises, validates and summarizes, but final approvals, contracts and legal decisions stay with people — and that boundary is enforced in code, not just stated in the paper. Integration with government agencies, banks, suppliers and accounting platforms was excluded as outside a capstone's verification ability. → [[Scope and Limitations]]

> [!question] Why is the decision support rule-based rather than a learning model?
> An analyst's answer, not a programmer's: the client needs to **justify** decisions to owners and clients, so every output must state its basis. A deterministic rule can be explained in a sentence and audited; a learned model at this data volume could not be either, and the firm has no training data of meaningful size. → [[Decision Support Overview]]

> [!question] How did you handle data that already exists on paper at ORRA?
> Describe your actual migration position honestly — if historical records are entered going forward rather than bulk-imported, say so and note it as a deployment recommendation.

> [!question] What assumptions did your analysis make?
> Name two or three plainly: that users have internet access at the office and on site, that roles map one-to-one to accounts, and that the firm's phase sequence stays stable. Stating assumptions unprompted is the single most analyst-like thing you can do in a defense.

> [!question] Does the manuscript match the system?
> Four known mismatches, and you should name them before the panel does: the decision support is rule-based rather than using external AI APIs; file storage is S3-compatible object storage rather than what the paper names; payroll **does** compute statutory employee shares despite an early limitation saying otherwise; and construction progress counts tasks *and* live milestones, not tasks alone. → [[00 Start Here]]

Related: [[Defense Questions Overview]] · [[The Problem at ORRA]] · [[Roles Overview]] · [[Lifecycle Gates]] · [[Scope and Limitations]]
