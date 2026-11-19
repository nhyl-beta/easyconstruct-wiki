---
tags: [easyconstruct, defense, questions, moc]
aliases: [Defense Questions, Panel Questions, Q&A, Mock Defense]
---
# 🎤 Defense Questions — Overview

> [!summary] 🟢 What this section is
> A bank of questions a panel is likely to ask about **EasyConstruct**, with a **short answer sketch** for each — grounded in what the system actually does, not what sounds good. Four kinds:
> - **[[General Questions]]** — the ones every panel asks every group (problem, objectives, scope, significance).
> - **[[Technical Questions]]** — stack, architecture, database, security, performance.
> - **Per-role** — what *you specifically* get asked because of your assigned role.

## The question sets
| Set | Use it when | Notes |
|---|---|---|
| [[General Questions]] | Everyone memorizes these | Opening 1-minute pitch lives here |
| [[Technical Questions]] | Anyone may be asked; Programmer answers most | Deepest follow-ups |
| [[Project Manager Questions]] | PM role | Team, timeline, scope, risk |
| [[Programmer Questions]] | Developer role | Code, algorithms, tooling |
| [[Quality Assurance Questions]] | QA role | Test strategy, bugs, evidence |
| [[System Analyst Questions]] | SA role | Requirements, modeling, business rules |
| [[Study Guide by Role]] 📚 | **Start here when revising** | What each member studies, plus a revision schedule |
| [[Paper-Based Questions]] 📄 | Questions straight from the manuscript | Title, Ch. 1–5, and where the paper contradicts itself |
| [[Developer Knowledge Check]] 🧠 | Fundamentals you're assumed to know | JWT, normalization, REST, CORS — asked mid-answer as traps |

## 🔵 How to use this bank
1. **Read the linked note, not just the answer.** Every answer points to the note with the real detail — the panel's second question is always *"can you expand on that?"*
2. **Answer in three beats:** *direct answer → one concrete piece of evidence → where it lives in the system.*
   > *"Yes — role access is enforced server-side. We have a test that reads every route's `requiredRoles` and compares it to the expected matrix; it runs on every build. See [[Access Control]]."*
3. **Never bluff.** "That's not implemented, and here's why it was out of scope" scores better than an invented feature. Your [[Scope and Limitations]] exists precisely so you can say that confidently.

> [!warning] The three questions most likely to hurt
> 1. **"Why do you call it AI when it's just if-else?"** → This one is **defensible — do not concede it.** Rule-based reasoning is a recognized branch of AI. The full argument and a scripted answer are in [[Why It Counts as AI]].
> 2. **"Did you deploy it at ORRA? What were the results?"** → Not yet implemented at ORRA, so there are no production or survey results. Say it plainly. → [[November 2026 Updates]]
> 3. **"Your manuscript says X but your system does Y."** → Eight documented contradictions inside the paper itself are listed in [[Paper-Based Questions#⚠️ Where the paper contradicts itself]], plus four manuscript-vs-code mismatches in [[00 Start Here]]. Know them before the panel finds them.

## 🟣 Preparation checklist
- [ ] Can deliver the 1-minute pitch without notes → [[General Questions]]
- [ ] Can draw the architecture diagram on a whiteboard from memory → [[Architecture Overview]]
- [ ] Can name all 10 roles and one unique thing each does → [[Roles Overview]]
- [ ] Can explain one gate check line by line → [[Lifecycle Gates]]
- [ ] Know your test numbers (1,090 API unit tests; 35 lifecycle cases, 191 steps; 901 role checks) → [[Testing Strategy]]
- [ ] Know the four manuscript-vs-code mismatches → [[00 Start Here]]
- [ ] Have a one-sentence answer for "what's your future work?"

Related: [[Objectives]] · [[Scope and Limitations]] · [[00 Start Here]]
