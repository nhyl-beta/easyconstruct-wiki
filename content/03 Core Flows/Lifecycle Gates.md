---
tags: [easyconstruct, flows, lifecycle, level/technical]
aliases: [Gates, Gate Checks]
---
# Lifecycle Gates

> [!summary] 🟢 In simple terms
> A **gate** is a checklist that must be all-green before a project can leave a phase. Each item names **who is responsible** for fixing it, so the right person sees it in their "Waiting on you" card.

Every check returns:
```ts
interface GateCheck {
  key: string;          // "P1", "C4", "X5"...
  label: string;        // "Architect staffed"
  ownerRoles: string[]; // who can fix it → drives "Waiting on you"
  passed: boolean;
  detail?: string;      // "2 of 3 proposal(s) approved"
  link?: string;        // where to go to fix it
}
```

## Construction path
| Phase to leave | Key | Check | Owner |
|---|---|---|---|
| **Proposal** | P1 | Architect staffed | PM |
| | P2 | Consultant staffed | PM |
| | P3 | Proposal submitted (linked to a workflow) | Architect |
| | P4 | Proposal approved (its workflow completed) | Consultant, PM |
| | P5 | Notice of Award **and** Contract on file, contract value > 0 | PM, Admin |
| **Design** | D1 | Design with files, assigned to a staffed engineer | Architect, PM |
| | D2 | All designs approved, no pending review | Consultant, PM |
| | D3 | Approved, current blueprint | Architect |
| **Pre-Construction** | C1 | Materials & specifications approved | Engineer, PM |
| | C2 | Budget approved (lines exist, all approved, total > 0) | Finance |
| | C3 | Milestones dated and active | PM |
| | C4 | Site crew staffed (active employees with logins) and linked to tasks | PM, Engineer, HR, IT Designer |
| | C5 | Notice to Proceed on file + site location set | PM |
| **Construction** | K1 | All tasks completed | Site Personnel |
| | K2 | All milestones closed | PM |
| | K3 | No open issues | Engineer, PM |
| | K4 | No active workflows | current stage role |
| **Closeout** | X1 | Final inspection approved | Engineer, PM |
| | X2 | Certificate of Completion on file | PM, Admin |
| | X3 | Closeout payroll approved | HR, Finance |
| | X4 | "Project Closeout" workflow completed | current stage role |
| | X5 | No open RFI/RFA | PM, Admin |
| | — | No unsettled procurement | Finance/PM |

## Design path
| Phase | Key | Check |
|---|---|---|
| Design | DP1 | Every plan set has a lead |
| | DP2 | Every plan set has a design with files |
| | D2, D3 | As above |
| | D4 | No open RFI/RFA |
| Turnover | T1 | Turnover document on file |
| | T2 | Client acceptance on file |
| | T3 | "Design Turnover" workflow completed |
| | T4 | No open RFI/RFA |

## 🟣 What a gate looks like in code
```ts
// apps/api/src/modules/projects/lifecycle/gates.ts
const p1 = (s: LifecycleSnapshot): GateCheck => {
  const has = s.members.some((m) => m.role === "architect");
  return {
    key: "P1",
    label: "Architect staffed",
    ownerRoles: ["project-manager"],
    passed: has,
    detail: has ? "Architect staffed" : "No architect staffed on this project",
    link: projectLink(s, ""),
  };
};
```
Reading it: *"Look at the project's members; if any has role architect, P1 passes. If not, tell the PM."*

> [!important] AI never touches gates
> Signals are appended to the lifecycle **view** only, never to `canAdvance`. `boundary.test.ts` fails the build if that ever changes. → [[Decision Support Overview]]

Related: [[Project Lifecycle]] · [[RFI and RFA]] · [[Payroll Engine]]
