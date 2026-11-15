---
tags: [easyconstruct, defense, questions, role/programmer]
aliases: [Developer Questions, Programmer Defense Questions, Coder Questions]
---
# Programmer / Developer — Defense Questions

> [!summary] 🟢 Who gets these
> The member who wrote the code. Panels test two things: **can you explain your own code without reading it**, and **did you make deliberate choices or copy a tutorial**. Specific file names and numbers are your best evidence — use them.

## Your choices
> [!question] What did you build, specifically?
> Name your actual areas rather than "the system." If you owned the API, say which of the nine modules — `iam`, `documents`, `projects`, `design`, `workflow`, `workforce`, `finance`, `insights`, `platform`. → [[API Modules]]

> [!question] Why did you structure the backend this way?
> Routes declare guards, controllers handle HTTP, services hold business rules, repositories hold SQL. The payoff is testability: because services never touch `req`/`res`, the gate logic, payroll engine and signal rules are all unit-tested as pure functions with no database. → [[Backend Layering]]

> [!question] Why TypeScript?
> Shared contracts between web and API in one repo — a change to a shared type breaks the build on both sides immediately. The project also maintains a **type-error baseline** in CI so the count can only go down, never up. → [[Monorepo Structure]]

> [!question] What third-party libraries did you use and why?
> Be ready to justify the main ones — Refine for the admin-style data layer, Drizzle for typed SQL, Tailwind with shadcn/ui for components, Playwright for end-to-end tests. For each: what it saved and what it cost. Never answer "because it's popular."

> [!question] What was the hardest part to implement?
> Pick something with real complexity and explain the difficulty, not just the feature. Strong candidates: the gate engine (many interdependent checks, each needing an owner role and a fix link), the payroll engine (statutory schedules plus a derivation trace), or scope-aware caching (cache keys must include visibility scope or data leaks between users). → [[Lifecycle Gates]] · [[Payroll Engine]] · [[Caching and Performance]]

---

## Explain your code
> [!question] Walk us through one function line by line.
> Have **one rehearsed**. The P1 gate check is ideal because it's short and complete: it reads the project's members, passes if any member has the architect role, returns a label, the owning role, a human-readable detail string, and a link to the page that fixes it. → [[Lifecycle Gates]]

> [!question] Explain the algorithm behind proposal validation.
> Similar past proposals of the same project type are collected, their median amount computed, and the new proposal flagged if it exceeds median × 2 or falls below × 0.4. Duplicate detection is separate: title similarity ≥ 0.7 with amount within ±10% inside 30 days. Cost-catalog matching uses the Dice coefficient at a 0.40 threshold. → [[Proposal Validation and Cost Reference]]

> [!question] How does requirement structuring work?
> A pure function splits a rough note into clauses, routes each into Objectives, Materials, Constraints or Specifications, rewrites a short list of vague phrases — **always reporting the rewrite rather than doing it silently** — and suggests commonly required elements the text never mentioned, drawn from the project-type template pack. → [[Decision Support Overview]]

> [!question] Where do the magic numbers live?
> Not scattered — thresholds are centralized in `core/config/signals.ts` so they can be tuned without touching logic. → [[Signals]]

> [!question] How does the frontend know what to show each role?
> A role-resources configuration lists pages per role, Refine's access control provider hides the rest, and a route guard blocks direct URLs. Say the next sentence unprompted: **this is UX only; the server enforces the real rule.** → [[Frontend Architecture]] · [[Access Control]]

---

## Quality & process
> [!question] How do you know your code works?
> Numbers: 1,090 API unit tests passing and 411 of 412 web tests as of 12 November, plus database tests against a real Postgres and an end-to-end suite. → [[Testing Strategy]]

> [!question] Tell us about a bug you caused and how you found it.
> The best one you have: on Linux CI, only **5 of 111** API test files were running because a glob wasn't quoted — the build was green while 95% of the tests never executed. The fix was quoting the globs, and the lesson was to check *how many* tests ran, not just the colour of the result. Panels respect this answer because it shows you verify your verification. → [[Testing Strategy]]

> [!question] How did you handle bugs reported by QA?
> In numbered batches with traceable IDs — B-1 through B-225 in the first week of November, then TC_2 (17 bugs) and TC_3 rounds — each fixed, retested and closed against its ID. → [[QA Bug Tracking Nov 2026]]

> [!question] How do you use version control?
> Separate branches by purpose: a staging branch for integration (`thesis-final-staging`), a dedicated branch for the QA automation work (`thesis-final-test`), with CI running tests and type checks on push. → [[Environments and Branches]]

> [!question] What did you do about security in the code?
> Passwords hashed with bcrypt, JWT bearer auth, server-side role guards on every protected route, a login rate limiter that resolves the client IP correctly behind the proxy, security headers, CORS rules — and all of those have unit tests (`throttle.test.ts`, `session.test.ts`, `security-headers.test.ts`, `cors.test.ts`). → [[Authentication and Sessions]] · [[Security Hardening Nov 2026]]

> [!question] Did you optimize anything for performance?
> Scope-aware caching on read paths, pagination on list endpoints, and a page-1 preview image generated at blueprint upload so document lists don't render heavy files. → [[Caching and Performance]]

> [!question] If a new developer joined tomorrow, how long until they could contribute?
> Point at the structure that makes the answer short: a consistent four-layer pattern in every module, a module manifest and registry, typed shared contracts, and tests that document expected behaviour. → [[Monorepo Structure]]

> [!question] What part of the code are you least happy with?
> Have an answer. Choosing a real weakness — a module you'd refactor, the type-error baseline you inherited rather than eliminated — reads as engineering maturity. "Nothing" reads as inexperience.

Related: [[Defense Questions Overview]] · [[Technical Questions]] · [[Backend Layering]] · [[Testing Strategy]]
