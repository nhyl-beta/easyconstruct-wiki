---
tags: [easyconstruct, big-picture, level/simple]
aliases: [Problem Statement, Project Context]
---
# The Problem at ORRA

> [!summary] 🟢 In simple terms
> ORRA is a 40-year-old construction and architecture firm (est. 1985). Their files sit in folders on a cloud drive that works like a **big USB stick**, not a management system. Schedules live in **Excel with colored cells**. Attendance and payroll are encoded by hand. So people waste time searching for files, schedules clash, and managers can't see the real status of a project.

## What the client said (interview with Mark Joseph Chapa, IT & designer of ORRA)
| Pain point | What happens today | What EasyConstruct does instead |
|---|---|---|
| File retrieval | Scan the whole storage manually to find archived files | Typed, searchable [[Documents and Files|document repository]] per project |
| Scheduling | Deadlines typed into Excel, progress shown by cell color | Dated milestones + tasks, calendar views, server-computed progress ([[Project Lifecycle]]) |
| Workforce | Staffing overlaps, allocation confusion | Project members, crew staffing gate (C4), workload warnings |
| Attendance & payroll | Manual records, repeated encoding, computation errors | [[Attendance and Geofencing]] → HR verification → [[Payroll Engine]] |
| Decisions | No consolidated view, delayed reporting cycles | Role dashboards, consolidated reports, [[Signals]] |

## Why it matters (from the literature review)
- Philippine construction grows fast but firms still rely on **fragmented manual methods** (Siman 2023; Jesus et al. 2025).
- Over 65% of local firms still use manual methods for records/payroll/tasks; ~70% are discouraged by **system complexity**, and >75% of managers want automation as **decision support, not replacement** (Jesus et al. 2025).
- That last finding shaped a core design rule: **every AI/rule feature is advisory; a person always decides** → [[Decision Support Overview]].

## Translating problems into design rules
1. **One system, many roles** → [[Roles Overview]], [[Access Control]]
2. **Nothing moves forward unless it's really done** → [[Lifecycle Gates]]
3. **Every important action leaves a trail** → audit logs ([[Security Hardening Nov 2026]])
4. **Numbers come from records, not typing** → progress, payroll and budgets are computed server-side
5. **Easy to use** → simplified sidebars, KPI cards, consistent tables (UI work in [[Development Timeline]])
