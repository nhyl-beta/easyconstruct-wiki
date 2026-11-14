---
tags: [easyconstruct, infrastructure, git]
aliases: [Branches, Git Flow, Staging]
---
# Environments and Branches

> [!summary] 🟢 In simple terms
> Code changes are tried on a **practice copy** first (staging branch) before going to the **real one** (thesis-final). A third branch holds the automated testing robot.

## Branches (Nov 2026)
| Branch | Purpose |
|---|---|
| `thesis-final` | Default branch; the thesis version; scheduled workflows run from here |
| `thesis-final-staging` | Where all new work lands first (latest: 13 Nov 2026) |
| `thesis-final-test` | QA automation suite (Playwright + CSV reporter), created 1 Nov |

Flow: feature work → `thesis-final-staging` → verified → merged to `thesis-final` → deployed.

## History of the branch model
`staging/dev/prod` (Sept–Oct) → enterprise refactor on `thesis-final` (Oct 19–20) → only `thesis-final` + `thesis-final-staging` (late Oct) → `thesis-final-test` added (Nov).

## Two versions of the product
- **Thesis version** — this repo/vault (EasyConstruct).
- **Client version** — a separate project (*Orra-ERP*), refactored weekly from client interviews toward a secure, enterprise file-management system, possibly running on ORRA's own server.

## Data safety rules
- Production DB host is guarded: QA and demo tools **refuse to run** against it.
- Demo data: `DEMO-STAGE-0 … 6` — one project per phase (8%, 25%, 29%, 59%, 99%, 100%, 100%) for presentations.

Related: [[Deployment and Hosting]] · [[Testing Strategy]] · [[Development Timeline]]
