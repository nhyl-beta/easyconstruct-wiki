---
tags: [easyconstruct, timeline, november-2026]
aliases: [November Files, Latest Changes, Nov 2026]
---
# November 2026 Updates (latest)

> [!summary] 🟢 In simple terms
> November was about **polish, proof and protection**: testers checked everything and bugs were fixed in batches, an automated test robot was built, a security review closed 13 holes, notifications became truly live, and the manuscript moved to Chapter 5.

Source: 74 commits on `thesis-final-staging` (1–13 Nov) + November project docs.

## Week 1 (1–7 Nov)
- **QA automation** branch `thesis-final-test` created (Playwright + CSV reporter). → [[Testing Strategy]]
- **Role decisions:** Admin may write on proposal submission, RFI/RFA, milestone completion; Site Personnel & Consultant upload documents; Owner can't create documents.
- **UI:** 260 px sidebar with hover groups; clickable KPI cards; shared filter bar; mobile branding; footer; read-only profile in Settings.
- **User Support** for every role (IT replies; Admin sees own; always last in sidebar).
- **Documents:** only Admin edits/deletes; document types Admin-only; IT keeps upload.
- **Activity logs:** full filter bar (date range, user, project, module, action), exact timestamps.
- Bug batches B-1…B-225 (access links, approval buttons, wrong numbers, filters, pagination).

## Week 2 (8–13 Nov)
- **TC_2** (17 bugs) and **TC_3** fixes; decisions N-4 (B) and N-5 (C). → [[QA Bug Tracking Nov 2026]]
- **Blueprints:** project → architect folder tree; only Consultant/PM/Admin approve; DWG/PDF upload; **page-1 preview image** generated at upload.
- **Designs:** Consultant can mark *In Review* as reviewed; delete works with review requests.
- **Proposals:** *Submitted by* and *Reviewer* filters; Failed validation card.
- **Users:** filters in hierarchy order, employee-ID search; unlinked accounts flagged.
- **Platform fixes:** 12 MB JSON only on uploads; login limiter uses `req.ip` (trusts proxy correctly); overdue sweeper uses transaction-level lock; **test globs quoted (5 → 111 files running)**; HR dashboard no longer shows made-up data.
- **12 Nov security & crash review** → [[Security Hardening Nov 2026]]
- **Neon Realtime** live query for the notification inbox; "Waiting on you" and approvals badge refresh live. → [[Notifications and Realtime]]
- **CI:** readiness check, boot job, live smoke test; demo reset counts a linked roster employee once.

## Manuscript status
- Objective 4 (reports) considered implemented; writing **Chapter 5** (findings, conclusions, recommendations).
- Implementation Results: **not yet implemented at ORRA** → no production/survey results reported.
- A user guide manual is being planned (role-based vs process-based structure).

Related: [[Development Timeline]] · [[Objectives]]
