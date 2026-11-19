---
tags: [easyconstruct, defense, questions, general]
aliases: [General Defense Questions, Common Questions, Panel Basics]
---
# General Questions (everyone must know these)

> [!summary] 🟢 In simple terms
> These are the questions asked in the first ten minutes of almost every defense. They're not about code — they're about **whether you understand your own problem**. Any member can be asked any of these.

## The 1-minute pitch (memorize this shape)
> [!example] Model answer
> *"EasyConstruct is a cloud-based construction operations system built for ORRA, a construction firm that runs projects on paper, spreadsheets and chat. Everything — proposals, designs, site tasks, attendance, payroll and approvals — lives in one place, with ten role-based modules so each person only sees and does what their job requires. Projects move through gated phases: the system won't let a project advance until the real-world requirements for that phase are actually satisfied. On top of that sits a decision-support layer that validates proposals, flags issues and forecasts the impact of changes — it advises, but people decide."*

Practise until it takes 45–60 seconds. Then stop talking.

---

## Problem & motivation
> [!question] What problem does your system solve?
> ORRA runs projects across disconnected tools — paper forms, spreadsheets, group chats. Information is late, duplicated or lost; approvals have no trail; attendance and payroll are manual; no one has a single view of project status. → [[The Problem at ORRA]]

> [!question] Why did you choose this topic?
> Tie it to the client, not to yourselves: a real firm with a real, observable workflow problem, close enough to interview repeatedly, and broad enough to exercise the whole IT curriculum (database, web, security, QA). Avoid "because it's interesting."

> [!question] Who benefits from this system?
> Name specific roles, not "everyone": the PM gets live project status and gate checks, site personnel log attendance from the field, HR and Finance stop re-typing data, and the Owner sees cycle times and approvals. → [[Roles Overview]]

> [!question] What makes your system different from existing ones (Procore, Buildertrend, manual systems)?
> Commercial platforms are priced and structured for large firms and assume mature processes. EasyConstruct is shaped around **ORRA's actual role structure and approval chain**, including phase gates derived from how they really work, and it's deployable on free-tier infrastructure. Don't claim to be better — claim to be **fitted**.

---

## Objectives & scope
> [!question] What are your objectives?
> Four, and know them cold: (1) a cloud-based unified system, (2) workforce management — attendance, labor cost, task allocation, (3) AI-driven project intelligence, (4) consolidated operational and analytical reports. → [[Objectives]]

> [!question] Were all your objectives met?
> Yes, with Objective 4 considered implemented as of November 2026. Be ready to point at the implementing module for each — the mapping table in [[Objectives]] is exactly that answer.

> [!question] Your proposal had eight objectives; now there are four. Why?
> They were **merged, not dropped** — the Chapter 1 proposal split the decision-support work into four separate objectives; the Capstone 2 manuscript consolidated them under one. Nothing promised was removed. → [[Objectives]]

> [!question] What is your scope and what are your limitations?
> Scope: ten role modules, each with login and dashboard, covering the full project lifecycle. Limitations: decision support advises but never approves; payroll is not a certified payroll engine and never remits; no live integration with government agencies, banks, suppliers or accounting platforms. → [[Scope and Limitations]]

> [!question] Why didn't you include [feature X]?
> The honest structure: *"It was out of scope because [reason tied to time, client priority, or the decision-support boundary]. It's listed in our recommendations."* Dropped during development: reimbursement claims, the employee self-service pay page, email copies of notifications. → [[Objectives]]

---

## On the "AI" claim
> [!warning] This is the question most likely to go badly. Rehearse it.

> [!question] You call it "AI-driven," but isn't it just rules and if-else statements?
> **Don't concede this one — it's defensible.** AI is not only machine learning; rule-based and knowledge-based reasoning is the oldest established branch of the field. Name the techniques: *"The signals module is a rule-based expert system — eight production rules, each with a conclusion and a recommended action. Issue precedents use case-based reasoning. Proposal validation is heuristic anomaly detection against the median of comparable past projects. Requirement structuring is rule-based NLP."*
> The full argument, the family-tree diagram and the scripted answer live in **[[Why It Counts as AI]]** — read that note before defense day.

> [!question] Why symbolic AI instead of machine learning?
> Three reasons, in this order: ORRA has dozens of historical records, not thousands, so there is nothing to train on; every output must be explainable because a PM has to justify a flag to an owner, and "twice the median of five comparable proposals" can be defended in a meeting while a model score cannot; and determinism makes it unit-testable, which is what lets us verify it within a capstone's scope. **Choosing not to use ML here is the correct engineering judgment.** → [[Why It Counts as AI]]

> [!question] Does your system call external AI APIs, as the System Flow paper says?
> **No — and concede this immediately.** Nothing in the codebase calls an AI model or service; the only outside data source is a read-only construction cost catalog. That line in the paper is a factual wording error and should be corrected. Keep the two issues separate: defend *"rule-based reasoning is AI"* firmly, and give up *"we call external AI APIs"* instantly. → [[00 Start Here]]

> [!question] Can you give a concrete example of what it actually does?
> Proposal validation: it finds past proposals of the same project type, takes the median amount, and flags the new one if it exceeds median × 2 or falls below × 0.4; separately it detects near-duplicates using title similarity ≥ 0.7 with amount within ±10% inside 30 days. Every flag names its comparison basis. → [[Proposal Validation and Cost Reference]]

> [!question] How do you guarantee the AI never makes the decision?
> There's an architectural boundary and a test that enforces it: signal output is attached to the lifecycle **view** only and can never reach `canAdvance`. `boundary.test.ts` fails the build if that rule is ever violated. → [[Lifecycle Gates]]

---

## Methodology & validation
> [!question] What methodology did you use?
> Iterative/Agile development against a fixed objective set, with work tracked as numbered fix lists per objective (e.g. *fix 3.2* = impact forecast) — the numbers still appear in code comments. Testing followed the manuscript's phases: Unit → Integration → Alpha → Beta → Field. → [[Testing Strategy]]

> [!question] How did you validate that the system works?
> Three independent layers: automated tests (1,090 API unit tests passing as of 12 Nov), a QA automation suite covering 35 lifecycle cases/191 steps and 901 per-role page checks, and manual testing by two testers recording results per role. → [[Testing Strategy]] · [[QA Bug Tracking Nov 2026]]

> [!question] Is it deployed at ORRA? What were the results?
> **No — not yet implemented at ORRA**, so there are no production usage results or user-satisfaction survey data to report. Say this plainly and move to what you *do* have: a live deployment, automated test evidence, and client-reviewed functionality. Inventing results here is the fastest way to fail. → [[November 2026 Updates]]

> [!question] Who were your respondents?
> Answer from your actual interview records (ORRA staff interviewed during requirements gathering) — see your `Interview.pdf` and `Interview_Transcript.pdf`. → [[System Analyst Questions]]

---

## Significance & future
> [!question] What is the contribution of your study?
> Not "a system." The contribution is a **role-gated project lifecycle model for a small construction firm**, where phase advancement is tied to verifiable artifacts (contract on file, budget approved, crew staffed) rather than to someone's say-so — plus an explainable decision-support layer that stays on the advisory side of the line.

> [!question] What are your recommendations / future work?
> Pick three and be specific: actual deployment and field validation at ORRA, integration with accounting or government remittance once certified, and a mobile-native client for site personnel. Avoid "add more AI."

> [!question] If you could start over, what would you do differently?
> A strong honest answer: *"We'd fix the terminology in the manuscript from the start, and we'd build the automated test suite earlier — we built it in the final month, and it immediately found problems manual testing had missed."* → [[Testing Strategy]]

> [!question] How much does it cost to run?
> It runs on free-tier services with no credit card required — static hosting, a managed Postgres, object storage, and scheduled jobs. That was a deliberate constraint so the client could adopt it without a budget line. → [[Deployment and Hosting]]

> [!question] Is the data secure? What about data privacy?
> Passwords are hashed, access is enforced server-side in three layers, sensitive actions are written to an audit trail, and the database is backed up on a schedule. A dedicated security review in November closed 13 findings. → [[Security Hardening Nov 2026]] · [[Access Control]]

Related: [[Defense Questions Overview]] · [[Paper-Based Questions]] · [[Technical Questions]] · [[Objectives]] · [[Scope and Limitations]]
