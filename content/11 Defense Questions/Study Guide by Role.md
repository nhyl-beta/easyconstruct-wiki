---
tags: [easyconstruct, defense, study-guide, roles]
aliases: [Study Guide, What to Study, Role Study List, Review Checklist]
---
# 📚 Study Guide — What Each Role Must Focus On

> [!summary] 🟢 How to use this
> Nobody can master all 59 notes. This splits the vault into **what everyone must know** and **what only your role must know deeply**. Study your column, skim the rest. Panels ask role-specific questions to the person holding that role — but they also ask a general question to anyone, so the shared core is not optional.

> [!important] The division of labour
> If a question lands on the wrong person, the right answer is *"[Name] handled that — may they answer?"* — not a guess. Deciding this **before** the defense is worth more than one extra night of reading.

---

## 🟩 Everyone — the non-negotiable core
Study these regardless of role. If any member can't do these, the whole group looks unprepared.

| # | Must be able to… | Source |
|---|---|---|
| 1 | Deliver the **1-minute pitch** without notes | [[General Questions]] |
| 2 | Name the **four objectives** and what implements each | [[Objectives]] |
| 3 | State the **scope and the three limitations** | [[Scope and Limitations]] |
| 4 | Name **all 10 roles** and one unique thing each does | [[Roles Overview]] |
| 5 | Describe the **project lifecycle phases** in order | [[Project Lifecycle]] |
| 6 | Explain **why it's called AI-driven** in two sentences | [[Why It Counts as AI]] |
| 7 | Say what **isn't** done: not deployed at ORRA, no field results | [[November 2026 Updates]] |
| 8 | Name the **four manuscript-vs-code mismatches** | [[00 Start Here]] |
| 9 | Draw the **architecture** as four boxes: browser → API → database → storage | [[Architecture Overview]] |
| 10 | Quote **one test number** (1,090 API unit tests) | [[Testing Strategy]] |
| 11 | Define any term you say out loud — JWT, ORM, REST, normalization | [[Developer Knowledge Check]] |
| 12 | Know the **ORRA facts**: founded 1985, three services, the server virus incident | [[Paper-Based Questions]] |

> [!tip] The 10-minute drill
> Each member answers all twelve out loud, timed, to the group. Anyone who hesitates on more than two re-reads those notes that night. Run it twice before defense day.

> [!warning] Before anything else: fix the paper
> [[Paper-Based Questions]] lists **eight places where the manuscript contradicts itself or the system** — future-tense testing, a conclusion that understates a deployed system, a Node version mismatch, a misspelled proponent name. Correcting those costs an afternoon and removes the easiest marks a panel can take from you.

---

## 🔵 Project Manager
> [!info] Your centre of gravity: **the lifecycle, the gates, and how the team was run.**

**Study deeply**
1. [[Lifecycle Gates]] — the single most important note for you. Know the **four construction-path gate groups** (P, D/C, K, X) and be able to recite the five Proposal checks P1–P5.
2. [[Project Lifecycle]] — phase order, both paths (Construction and Design), what progress counts.
3. [[Project Manager]] — the role's actual permissions in the system.
4. [[Workflows and Approvals]] — who approves what, and the override rule.
5. [[Project Manager Questions]] — your question set.

**Memorize cold**
- The **P1 gate, line by line** — it's the one you'll be asked to walk through. → [[Lifecycle Gates]]
- **Approval authority:** only this project's PM; Admin override needs a reason ≥ 10 characters, written to the audit trail.
- **Scope changes:** added RFI/RFA, Design project type, procurement, owner email approvals, support requests; cut reimbursement claims, employee self-service pay page, notification email copies.
- **74 commits** on the staging branch, 1–13 November.
- `ownerRoles` on a failing gate is what drives each person's **"Waiting on you"** card.

**Be ready to do**
- Draw the lifecycle phases on a whiteboard with one gate check written under each.
- Answer *"how did you handle scope creep?"* with the add/cut lists above — not with adjectives.

> [!warning] Your danger zone
> *"Did you follow your schedule?"* — never claim a perfect timeline. Name one real slip, what you resequenced, what shipped anyway.

---

## 🟣 Programmer / Developer
> [!info] Your centre of gravity: **explaining your own code without reading it.**

**Study deeply**
1. [[Backend Layering]] — routes → controller → service → repository, and *why* (testability).
2. [[Request Journey]] — one click, end to end. Rehearse narrating it.
3. [[Lifecycle Gates]] (the 🟣 code block) — your one line-by-line walkthrough.
4. [[Access Control]] — the three layers; you'll be asked how data is protected.
5. [[Database and Drizzle]] · [[Monorepo Structure]] · [[Tech Stack]] — the "why this stack" answers.
6. [[Programmer Questions]] · [[Technical Questions]] — your question sets.

**Memorize cold**
- The **nine API modules**: iam, documents, projects, design, workflow, workforce, finance, insights, platform.
- **Why PERN:** one language end to end, shared types, relational domain needing transactions.
- **Why Drizzle:** typed queries compiling to predictable SQL, plain readable SQL migrations.
- **Dice ≥ 0.40** (cost catalog), **title similarity ≥ 0.7 + amount ±10% within 30 days** (duplicates), **median × 2 / × 0.4** (outliers).
- Thresholds live in **one file** — `core/config/signals.ts`.
- Security: bcrypt, JWT, server-side role guards, rate limiter using `req.ip` correctly behind proxy.

**Be ready to do**
- Walk through the **P1 gate function** line by line, from memory.
- Explain one algorithm end to end — proposal validation is the best choice.
- Answer *"what's your weakest code?"* with a real answer. "Nothing" reads as inexperience.

> [!warning] Your danger zone
> *"The frontend hides buttons by role — isn't that insecure?"* Answer in one breath: **frontend hiding is UX only; the server is the real lock, every route carries its own guard, and a test verifies every route against the expected matrix.**

---

## 🟠 Quality Assurance
> [!info] Your centre of gravity: **numbers and evidence.** You have more hard figures than anyone — lead with them.

**Study deeply**
1. [[Testing Phases Explained]] — start here, especially *levels vs phases*.
2. [[Testing Strategy]] — the pyramid, the CSV specs, the automation guard.
3. [[QA Bug Tracking Nov 2026]] — the bug cycle, the batches, what's still open.
4. [[Access Control]] — because most of your defects were role-dependent.
5. [[Quality Assurance Questions]] — your question set.

**Memorize cold**

| Evidence | Figure |
|---|---|
| API unit tests passing | **1,090** |
| Web tests | **411 of 412** |
| API test files | **111** |
| Lifecycle cases / steps | **35 / 191** |
| Role page checks | **901** |
| Access matrix | pages × **10 roles** (Full / View only / Deny) |
| Manual testers | **2** |
| Manual pass duration | ~**36 hours** of two-person clicking |
| November bug batches | **B-1 … B-225**, then TC_2 (17 bugs), TC_3 |

**Be ready to do**
- Tell the **5-of-111 false-green story** in under a minute, ending on the lesson: *check how many tests ran, not just that it was green.*
- Explain the **independent progress oracle** — the test computes progress itself rather than trusting the app.
- List what you did **not** test (load/stress at scale, long-term data growth, real field conditions) **before** being asked.

> [!warning] Your danger zone
> *"What's your coverage percentage?"* — if you don't have a measured number, don't invent one. Describe coverage **by risk** instead: every role's access matrix, every gate, payroll, signals, auth.

---

## 🔴 System Analyst
> [!info] Your centre of gravity: **proving the design came from the client, not from imagination.**

**Study deeply**
1. [[The Problem at ORRA]] — the as-is process; this is your foundation.
2. [[Roles Overview]] — why ten roles, grouped by function.
3. [[Lifecycle Gates]] — your business rules *are* the gates; this is your best artifact.
4. [[Scope and Limitations]] — how the boundary was decided.
5. [[Database and Drizzle]] — enough to redraw the core ERD entities.
6. [[System Analyst Questions]] — your question set.

**Memorize cold**
- **The role grouping:** oversight (Owner, Admin, IT Designer) · delivery (PM, Engineer, Site Personnel) · design (Architect, Consultant) · back office (HR, Finance).
- **The Super Admin split:** one account was doing two jobs → Owner (business oversight) + IT Designer (system administration); Admin became operational.
- **One requirement traced end to end:** pain → design decision → gate check → code → test. Use the "is this project really ready?" → phase gates thread.
- Your **interview sources** and who was interviewed, by function.
- The **four manuscript mismatches** — you should be the one who names them first.

**Be ready to do**
- Redraw the **core ERD entities**: projects, members, proposals, designs, milestones, tasks, workflows, attendance, payroll.
- Describe **as-is vs to-be** in four sentences.
- State your **assumptions** unprompted (internet on site, one account per role, stable phase sequence) — the most analyst-like move available to you.

> [!warning] Your danger zone
> *"Where are your business rules documented?"* Don't point only at the paper. The gates **are** the documentation — key, label, owner roles, pass condition — which is why specification and implementation can't drift apart.

---

## 🗓️ Suggested revision schedule
| When | Everyone | Per role |
|---|---|---|
| **1 week out** | Read the 10 core items | Read your 5–6 deep notes once |
| **3 days out** | Run the 10-minute drill | Memorize your "cold" list |
| **2 days out** | Mock defense: each member answers 5 questions from their set | Rehearse your walkthrough (gate / code / story / ERD) |
| **1 day out** | Re-read [[Why It Counts as AI]] + the four mismatches | Skim your question set once |
| **Morning of** | 1-minute pitch, out loud, twice | One number you'll say in your first answer |

> [!tip] The last thing to agree on
> Decide **who answers what** before you walk in, and decide who the group's *closer* is — the person who takes "any final clarification?" Confusion about who speaks costs more marks than a weak answer delivered confidently.

Related: [[Defense Questions Overview]] · [[General Questions]] · [[Technical Questions]] · [[Testing Phases Explained]] · [[Why It Counts as AI]]
