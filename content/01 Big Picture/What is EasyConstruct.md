---
tags: [easyconstruct, big-picture, level/simple]
aliases: [EasyConstruct, Overview]
---
# What is EasyConstruct?

> [!summary] 🟢 In simple terms
> EasyConstruct is a **website that runs a construction company's work in one place**. Instead of Excel sheets, shared folders and chat messages, every person in the company (owner, managers, HR, finance, architects, engineers, site workers, consultants) logs in and sees **only the screens for their job**. A project moves step by step from *proposal* to *finished building*, and the system checks that each step is really done before the project can move on.

## The one-sentence version
A **cloud-based, role-based construction operations management system** with rule-based decision support, built as a thesis for **ORRA Construction Inc.** (Oscar R. Ruivivar & Associates) at STI College San Jose del Monte.

Full manuscript title: *EasyConstruct: A Cloud-Based Construction Operations Management System with AI-Driven Proposal Validation, Workflow Automation, and Impact Awareness for Orra Construction Inc.*

## An analogy 🏗️
Think of EasyConstruct as an **airport**:
- **Passengers** = projects. They go through fixed checkpoints in order → [[Project Lifecycle]].
- **Checkpoint officers** = [[Roles Overview|roles]]. Each officer can only stamp their own checkpoint → [[Access Control]].
- **Gate checks** = boarding passes. You cannot board until every item is ticked → [[Lifecycle Gates]].
- **Control tower** = Owner and Admin dashboards watching everything.
- **Radar warnings** = [[Signals]] that say "this flight looks delayed" but never fly the plane for you → [[Decision Support Overview]].

## What it covers
| Area | What it does | Note |
|---|---|---|
| Projects | Proposal → Design → Pre-Construction → Construction → Closeout → Completed → Archived | [[Project Lifecycle]] |
| Approvals | Multi-step sign-offs (budget change, change order, closeout…) | [[Workflows and Approvals]] |
| Design | Proposals, designs, reviews, blueprints, plan sets, RFI/RFA | [[RFI and RFA]] |
| Site | Geofenced attendance with live camera photo, tasks, issues | [[Attendance and Geofencing]] |
| People & pay | Employee records, attendance-based payroll with PH statutory deductions | [[Payroll Engine]] |
| Money | Budgets, expenses, purchase requests, procurement, cash flow | [[Finance Module]] |
| Files | Versioned uploads, SHA-256 integrity, audited downloads | [[Documents and Files]] |
| Insight | Rule-based signals, proposal checks, plan drafts, impact forecasts | [[Decision Support Overview]] |
| Platform | Notifications (live), audit logs, quick search, scheduled jobs | [[Notifications and Realtime]] |

## How big is it? (Nov 2026, `thesis-final-staging`)
- ~143,000 lines of TypeScript across `apps/web` and `apps/api`
- 9 backend modules, ~44 database schema files, 47 SQL migrations → [[Database and Drizzle]]
- 10 roles → [[Roles Overview]]
- ~108 API test files and ~39 web test files (1,090 API unit tests passing on 12 Nov) → [[Testing Strategy]]
- Live at **easyconstruct.online** on free-tier cloud services → [[Deployment and Hosting]]

## Where to go next
1. Why it exists → [[The Problem at ORRA]]
2. What it promised → [[Objectives]] and [[Scope and Limitations]]
3. How it is built → [[Architecture Overview]]
