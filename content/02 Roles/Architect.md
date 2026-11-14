---
tags: [easyconstruct, roles, role/architect]
---
# Architect
> [!summary] 🟢 Simple
> Draws and submits the **designs**. Starts the project's proposal, uploads blueprints, makes revisions when the consultant asks, and answers technical questions (RFIs).

## Features (scope)
Dashboard · Proposal Submission (with rule-based validation) · Design Submission · Revision Management · Blueprint Management (project → architect folder tree) · Document Management · Approvals (Design Proposal Approval, Public works compliance) · Project Monitoring · Impact Awareness Viewing · Requests (answer RFI/RFA)

## 🔵 Technical notes
- Gates owned: **P3** (proposal submitted), **D1** (design with files), **D3** (approved current blueprint), **DP1/DP2/T1** on Design projects.
- Proposals get automatic **validation** (amount vs median of same project type, duplicate detection) → [[Proposal Validation and Cost Reference]]
- Blueprints accept PDF/DWG; PDF cards show a **page-1 preview image** generated at upload (Nov 11).
- Sending a revision on a *Revision Required* design re-opens it and **queues a new Consultant review** (B-211).
- Only Consultant/PM/Admin **approve** blueprints; the architect submits.
- Membership-scoped: sees only projects they're staffed on. → [[Access Control]]

Related: [[Consultant]] · [[RFI and RFA]]
