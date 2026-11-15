---
tags: [easyconstruct, defense, questions, role/qa]
aliases: [QA Questions, Tester Questions, Quality Assurance Defense Questions]
---
# Quality Assurance — Defense Questions

> [!summary] 🟢 Who gets these
> The member responsible for testing. Your advantage over every other role: **you have numbers**. Use them in the first sentence of each answer — panels rarely challenge a specific count.

## The numbers to memorize
| Evidence | Figure |
|---|---|
| API unit tests passing (12 Nov) | **1,090** |
| Web tests passing | **411 of 412** |
| API test files | **111** |
| Lifecycle test cases / steps | **35 cases, 191 steps** |
| Role page checks | **901** |
| Access matrix | pages & actions × **10 roles** |
| Bug batches (Nov week 1) | **B-1 … B-225** |
| Manual testers | **2** |

---

## Strategy
> [!question] What testing did you perform?
> Four levels, bottom to top: unit tests on pure functions, database tests against a real Postgres, end-to-end UI tests in Playwright, and manual testing by two testers recording results in per-role sheets. Describe it as a pyramid — many fast tests at the bottom, few slow ones at the top. → [[Testing Strategy]]

> [!question] How does that map to the testing phases in our manuscript?
> Unit → Integration (for example, a Site Personnel attendance record reaching HR and then payroll) → Alpha → Beta with ORRA office staff → Field testing. → [[Testing Strategy]]

> [!question] How did you design your test cases?
> From specifications, kept as CSV files that are the source of truth: an access matrix of pages and actions against all ten roles marked Full / View only / Deny, 35 lifecycle test cases broken into 191 steps, and 901 role page checks covering navigation, content, search and filter, pagination, form validation, read-only enforcement and denial. → [[Testing Strategy]]

> [!question] Why automate? You had manual testers.
> Because the matrix was too large for two people — roughly 36 hours of two-person clicking per full pass, which can't be repeated after every bug fix. Automation reduced that to a short human review and made **regression** testing possible at all. → [[Testing Strategy]]

> [!question] What's the difference between your manual and automated results?
> They use the **same report format** — the automated CSV reporter emits the same columns as the manual sheet (Test Case ID, Objective, … Status, Remarks), with "Executed by" recorded as *Automated (Playwright)*. That's deliberate: the two are directly comparable. → [[Testing Strategy]]

---

## Defects
> [!question] How did you track bugs?
> Numbered IDs in batches — B-1 through B-225 in early November, then TC_2 with 17 bugs and a TC_3 round — each traced from report to fix to retest. → [[QA Bug Tracking Nov 2026]]

> [!question] What kinds of bugs did you find most?
> Name the real categories: broken access links, approval buttons behaving wrongly, incorrect displayed numbers, and filter and pagination faults. Then note the pattern — most were **role-dependent**, which is exactly why the access matrix became the backbone of the suite. → [[QA Bug Tracking Nov 2026]]

> [!question] What's the most serious bug you found?
> Strong answer: the test runner itself. On Linux CI only **5 of 111** API test files were executing because a glob wasn't quoted — so the build reported green while 95% of the suite never ran. Finding it meant every prior "passing" build had to be re-evaluated. The lesson: verify the count of tests executed, not just the status. → [[Testing Strategy]]

> [!question] How do you classify severity?
> Explain your actual practice: blockers (role can't complete their core task or data is wrong), major (feature misbehaves with a workaround), minor (cosmetic or copy). Tie prioritization to the role affected — a bug blocking the PM's approval path outranks a misaligned label.

> [!question] Are there still open bugs?
> Answer honestly from your tracker. "Yes, N known issues, all minor, documented with workarounds" is a professional answer; "none" invites the panel to find one live. → [[QA Bug Tracking Nov 2026]]

> [!question] How do you prevent fixed bugs from coming back?
> Each fixed defect's scenario goes into the automated suite, so it runs on every subsequent build — that's what makes the suite grow in value over time.

---

## Coverage & limits
> [!question] How do you test role-based access, specifically?
> The access matrix CSV drives automated API permission checks, and separately a unit test reads every route's declared `requiredRoles` and compares them to the expected matrix **without needing a database** — so an accidentally unguarded route fails the build. → [[Access Control]]

> [!question] What is your test coverage?
> If you have a coverage number, give it; if not, describe coverage by **risk** instead: every role's access matrix, every lifecycle gate, the payroll engine, the signal rules, auth and security headers are all covered. Don't invent a percentage.

> [!question] What did you NOT test, and why?
> Good honest answers: load and stress testing at realistic concurrent-user volumes, long-term data growth, and real-world field conditions at ORRA — because the system **has not been deployed there yet**. Listing untested areas yourself is far stronger than being caught by the question. → [[November 2026 Updates]]

> [!question] How did you manage test data?
> Seeded demo accounts for CI and a demo reset routine; one detail worth citing is that the reset counts a linked roster employee exactly once, which was itself a bug found and fixed. → [[Testing Strategy]]

> [!question] Could your automated tests damage real data?
> No — there's an explicit guard: the suite refuses to run against the production database host or the live domain. → [[Testing Strategy]]

> [!question] How did you verify the system's own calculations rather than just its screens?
> The lifecycle suite uses an **independent progress oracle** — progress is computed separately by the test and compared against what the system reports, so the test doesn't simply trust the application's own output. → [[Testing Strategy]]

Related: [[Defense Questions Overview]] · [[Testing Strategy]] · [[QA Bug Tracking Nov 2026]] · [[Access Control]]
