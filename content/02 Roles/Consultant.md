---
tags: [easyconstruct, roles, role/consultant]
---
# Consultant
> [!summary] 🟢 Simple
> An **outside expert reviewer**. Checks the architect's proposals and designs, approves or asks for changes, and uploads advisory reports.

## Features (scope)
Dashboard (reviews, overdue card, calendar) · Proposal Review (with validation summary) · Design Reviews (approve / request changes / reject) · Design Repository · Advisory Document Upload · Project Viewing · Requests (answer RFI/RFA)

## 🔵 Technical notes
- Second stage of **Design Proposal Approval** and **Design Turnover**; first stage of **Document Compliance Review**.
- Gates owned: **P4** (proposal approved, with PM) and **D2** (all designs approved).
- Design review decisions moved from Architect to Consultant (server enforces `requireRole('consultant','project-manager','admin')`).
- Can move plan sets to `approved` or back to `in_progress`.
- Membership-scoped access. → [[Access Control]]

Related: [[Architect]] · [[Workflows and Approvals]]
