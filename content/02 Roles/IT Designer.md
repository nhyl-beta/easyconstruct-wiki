---
tags: [easyconstruct, roles, role/it-designer]
---
# IT Designer
> [!summary] 🟢 Simple
> The **system administrator**. Creates user accounts, sets roles, watches security, answers support tickets. Doesn't run projects.

## Features (scope)
Dashboard (incl. AI impact reports) · User Account Management · Roles & Permissions (manage) · Security Monitoring · Activity Logs · Workflow Configuration · Approval Hierarchy · Document Repository (upload) · Proposal Monitoring · Project Operations (monitor) · System Oversight · User Support (replies) · AI Validation Reference page

## 🔵 Technical notes
- **Every account is linked to an employee record** (one account ↔ at most one employee, unique index). HR creates employees; IT creates the login (fix B-78).
- A **Configuration check** flags unlinked accounts and lists them in "Waiting on you".
- Account creation and actions on the Owner account need **Owner email approval**. → [[Owner]]
- **Changing someone's role signs them out** (security fix #13). Password change or deactivation also ends sessions. → [[Authentication and Sessions]]
- Project write routes deliberately exclude IT Designer: "system administration, not project delivery".
- Cannot see pay rates or attendance data.

Related: [[Access Control]] · [[Roles Overview]]
