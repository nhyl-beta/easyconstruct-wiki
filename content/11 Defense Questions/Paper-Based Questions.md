---
tags: [easyconstruct, defense, questions, manuscript]
aliases: [Manuscript Questions, Paper Questions, Chapter Questions, Capstone Paper Questions]
---
# Paper-Based Questions (Capstone 2 manuscript, Nov 2026)

> [!summary] 🟢 What this is
> Questions drawn **straight from the manuscript** — the title, Chapter 1 context, the literature review, the methodology, and the Results chapter. The panel has read this document; they will quote it back at you. Every fact below is from the paper itself, so if an answer here contradicts what you remember, the paper wins and you should fix the paper.

> [!warning] Read the last section first
> **[[#⚠️ Where the paper contradicts itself]]** lists the internal inconsistencies a sharp panelist will find. Fixing those before defense day is worth more than memorizing any answer on this page.

---

## The title — defend it word by word
Your title makes four claims. Expect one question per phrase.

> *"EasyConstruct: A Cloud-Based Construction Operations Management System with **AI-Driven Proposal Validation**, **Workflow Automation**, and **Impact Awareness** for ORRA Construction Inc."*

> [!question] Why "cloud-based"? Isn't it just a website?
> Cloud-based describes where it runs and how it is reached: a managed Postgres database, a containerized API, static hosting on a CDN, and object storage — none of it on ORRA's premises, all reachable from the office and from site. It replaces the client's local physical server, which was their actual pain point. → [[Deployment and Hosting]]

> [!question] What exactly is "proposal validation"?
> Automatic checks on a submitted proposal before a human reviews it: comparison against similar past proposals of the same type (flagged if above median × 2 or below × 0.4), duplicate detection, contract cross-check, and line-item matching against a construction cost catalog. It produces a flag and a reason, never a decision. → [[Proposal Validation and Cost Reference]]

> [!question] What does "workflow automation" mean in your system?
> Approval routing: a transaction enters a workflow with defined stages and assigned approvers, moves automatically to the next stage on approval, notifies the right person, and records every action. "Automation" is the routing and notification, not the decision. → [[Workflows and Approvals]]

> [!question] What is "impact awareness"?
> Before approving a change, the approver sees the projected consequence: cost against contract value, estimate at completion, and schedule slip in days. The paper's own example is an architect adding a floor — cost up, timeline up, more workers needed, with a recommendation to review budget allocation before approving. → [[Plan Suggestion and Impact Forecast]]

---

## Chapter 1 — Project Context
The panel will test whether you actually know your client. These facts are all in your paper.

> [!info] The ORRA facts to have at your fingertips
> | Fact | Detail |
> |---|---|
> | Full name | **Oscar R. Ruivivar & Associates (ORRA)** |
> | Established | **1985** — 40 years in operation |
> | Services | **DAED** (Detailed Architectural and Engineering Design) · Design and Project Management · Construction Services |
> | Construction arm | **RuVi Construction Co. Inc.** — separate but directly affiliated |
> | Client load | **4–5 smaller private clients annually**; government contracts up to **8 years** |
> | Payroll today | Manually computed in **Excel**, repeated data entry |
> | Attendance today | **DTRs** kept by foremen or PMs, encoded weekly in the office |
> | File storage today | A **local physical server** used as a folder-based repository, searched manually |

> [!question] What are ORRA's three main problems?
> The paper names them as project monitoring, scheduling, and document retrieval: tasks assigned late or finished past agreed timelines; no real-time central monitoring, so management can't see accomplishments, pending work, or delays; scheduling conflicts when staff are assigned to multiple distant sites; and slow retrieval from growing manual folders and physical records.

> [!question] Tell us about the server incident.
> A previous employee installed unauthorized software that introduced a virus and corrupted several folders. Passwords were lost during turnover, making parts of the server inaccessible, so **digital files from before 2019 cannot be reached** and many older projects exist only as physical records.
> **Why this matters in the defense:** it is your strongest justification for cloud hosting, role-based access, audit trails and automated backups — your client has already lived through the failure those features prevent. → [[Scheduled Jobs and Backups]] · [[Access Control]]

> [!question] Why does ORRA's business structure matter to your design?
> Because two of their three services involve **design without construction**. That is exactly why the system has a separate **Design project type with a Turnover phase**, rather than forcing every project down the construction path. → [[Project Lifecycle]]

---

## Chapter 2 — Related literature and systems
> [!question] What existing systems did you review, and why aren't they enough?
> **Procore** and **Autodesk Construction Cloud** (cloud documentation, scheduling, collaboration), plus local options **BuildIT** and **Monark CAT** (monitoring, equipment tracking, reporting). The paper's argument is that they concentrate on project execution, technical coordination or asset management, and give limited support for **integrated payroll, attendance, workflow orchestration, proposal recommendation, and administrative decision support** aligned to a medium-sized Philippine firm.

> [!question] Name some of your sources.
> Have four ready. The Philippine-context ones are the most useful: **Siman (2023)** on the Philippine construction industry; **Alviar et al. (2024)** on mobile technology for construction project management in the Philippine setting; **Canivel et al. (2024)** on an automated dashboard for construction progress monitoring; **Dimaculangan (2023)** on issues and challenges in the Philippine construction industry. For the AI and cloud side: **Wang et al. (2024)**, **Ali & Zeebaree (2025)**, **Al-Sharafi et al. (2023)** on AI-driven decision support for SMEs.

> [!question] What is the research gap your study fills?
> Existing platforms stop at documentation and monitoring. The gap is a **single platform that combines administrative operations (payroll, attendance, HR, finance) with project operations and explainable decision support**, scaled and priced for a medium-sized Philippine firm.

> [!question] How does your study relate to the literature on AI in construction?
> The literature reports that AI and cloud computing reduce delays and errors and support evidence-based decisions; your contribution is applying that at the scale of a firm that has **no data science capability and no training data** — which is precisely why the implementation is rule-based. → [[Why It Counts as AI]]

---

## Chapter 3 — Methodology
> [!question] Why did you choose these technologies?
> Group the answer rather than listing twenty names: TypeScript end to end for shared types; React with Refine for data-heavy role dashboards; Express and Node for the API; PostgreSQL with Drizzle for relational integrity and typed queries; and managed cloud services chosen so the client pays nothing to run it. → [[Tech Stack]]

> [!question] Several technologies in your paper aren't in your architecture diagram. What do they do?
> | Technology | Its job |
> |---|---|
> | **Leaflet + OpenStreetMap** | Displays project sites and draws the geofenced areas used for site attendance — free, no paid map service → [[Attendance and Geofencing]] |
> | **Zod** | Validates the shape of incoming request data before it is processed or stored |
> | **TanStack Query** | Caches and refreshes server data on the client so dashboards and tables load faster |
> | **Recharts** | Dashboard charts — progress, attendance, payroll, cash flow |
> | **Upstash Redis** | Shared cache for repeated reads, with automatic in-memory fallback → [[Caching and Performance]] |
> | **Docker** | Packages the API so it runs identically in development and deployment |
> | **UptimeRobot** | Hits the API health endpoint every 5 minutes, keeping the hosted API awake and alerting the developers when it is unreachable |
> | **bcryptjs / JWT** | Password hashing and token authentication → [[Authentication and Sessions]] |

> [!question] How did you gather requirements?
> Client interviews at ORRA's office — the first on **24 February 2026**, a second on **17 April 2026** — plus adviser consultations roughly weekly. Interview findings were written back into the project context and the scope. → [[System Analyst Questions]]

> [!question] Your paper includes a hardware and software table. Why does that matter?
> It documents the development environment so the work is reproducible — the machines the system was built and tested on, and the pinned versions of the major tools. Be ready for the follow-up: *"will it run on weaker hardware?"* The answer is yes for users, since it's a web application; the specifications describe the **development** machines, not a client requirement.

---

## Chapter 4 — Results and Discussion
> [!question] Walk us through your testing.
> Five levels in the paper: **unit** (each role module examined individually), **integration** (data crossing module boundaries), **alpha** (controlled testing by the developers), **beta** (selected ORRA office personnel), and **field** (actual or simulated working environment). Give the standing integration example — Site Personnel records attendance → HR verifies it → payroll computes from it. → [[Testing Phases Explained]]

> [!question] Which integrations did you specifically test?
> The paper names them: Site Personnel → HR attendance; Project Management → Document Management → Workflow Orchestration; HR → Payroll; Finance → Payroll; Project Management → decision support; and Engineer requirements → AI-assisted requirement drafting.

> [!question] What is the state of your prototype?
> Interconnected modules for administration, project management, HR, finance, architecture, engineering, consultancy, site operations, documents, reporting and decision support, with role-based access across all ten roles.

> [!question] What are your implementation results?
> **There are none to report, and say so directly.** The paper states that EasyConstruct has not yet been implemented in ORRA's operational environment, so no deployment results, production performance figures, or post-implementation user evaluation are reported. Then pivot to the evidence you *do* have: automated test results and client-validated functionality. → [[Testing Strategy]]

> [!question] Your implementation plan — what are the stages?
> Finalize configuration (database, modules, interfaces, role-based access) → deploy to secure cloud hosting with production server, database, security setup and user accounts, with optional initial data migration → user orientation → initial monitoring → review of performance and feedback.

---

## Chapter 5 — Conclusion
> [!question] What did your study conclude?
> That the system's design, architecture and core functionality were successfully established from the client's requirements — while noting that further development, testing and evaluation remain necessary to assess functionality, usability, reliability and effectiveness.

> [!question] Why is your conclusion so cautious when the system is already running?
> This is the question hiding in your own conclusion — see the next section. Decide as a group how you will answer it **before** the defense.

---

## ⚠️ Where the paper contradicts itself
> [!warning] Find these before the panel does
> A panelist who catches an inconsistency you haven't noticed will keep pulling that thread. A group that says *"yes, we found that, here's the corrected version"* loses almost nothing.

| # | The problem | Where | How to handle it |
|---|---|---|---|
| 1 | **Testing is written in future tense.** Beta and field testing say *"will conduct"*, as though nothing has been done — but ~1,090 API unit tests pass, and an automated suite covers 35 lifecycle cases and 901 role checks. | Ch. 4, Testing | Rewrite unit/integration/alpha in past tense with the actual numbers; keep beta and field in future tense, since those genuinely haven't happened. |
| 2 | **The conclusion understates the system.** It says the system *"remains under development…with several features still undergoing implementation"*, while the system is deployed and feature-complete against all four objectives. | Ch. 5 | Rewrite to: objectives met and system deployed; **not yet rolled out to the client's operations**, so effectiveness in production is still to be evaluated. That is both accurate and still appropriately humble. |
| 3 | **Node version mismatch.** The paper's software table says **Node.js 26.10.0**; the repository README says **Node 18+, tested on Node 22**. | Ch. 3 table vs README | Make them agree. State the version you actually build and run on. |
| 4 | **"AI-driven" is in the title**, but the implementation is rule-based and calls no AI service — and the System Flow document implies external AI APIs. | Title, Ch. 3, System Flow | Keep the title; define the mechanism in Chapter 3 as **rule-based / knowledge-based decision support**, and correct the external-API wording. Full argument: [[Why It Counts as AI]] |
| 5 | **Abstract says "Date of Completion: June 2027."** | Abstract | Confirm this is your intended graduation date and not a typo; a panelist may read it as the system being unfinished. |
| 6 | **Name typo**: the endorsement page reads *"Marc Jocan C. Bercero"*; everywhere else it is *"Marc Jovan C. Bercero"*. | Endorsement form | Fix before submission — a misspelled proponent name on a signed form is an avoidable embarrassment. |
| 7 | **Spelling errors in technical text**: "EasyCOnstruct", "dahsboards", "helath endpoint". | Ch. 3, Development | Proofread pass over the technical sections. |
| 8 | **Progress counting.** Construction progress counts tasks *and* live milestones in the code; earlier documents describe tasks alone. | System Flow | → [[00 Start Here]] |

> [!tip] If a panelist finds one you haven't fixed
> *"You're right — that's an error in the manuscript. The system does X; the document says Y. We'll correct it in the final version."* Then stop. Don't improvise a justification for a mistake.

Related: [[Defense Questions Overview]] · [[General Questions]] · [[Technical Questions]] · [[Why It Counts as AI]] · [[Study Guide by Role]]
