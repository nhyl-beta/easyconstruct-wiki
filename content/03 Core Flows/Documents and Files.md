---
tags: [easyconstruct, flows, documents, storage]
aliases: [Documents, Document Repository, Uploads, File Storage]
---
# Documents and Files

> [!summary] 🟢 In simple terms
> All project files (contracts, permits, blueprints, photos) live in **one online storage bucket**, and the database keeps a **"library card"** for each file (title, type, project, version, who uploaded it). Every download is logged, and the system can prove a file **hasn't been changed** since it was uploaded.

## 🔵 Two parts: bytes vs records
| Part | Where | Example |
|---|---|---|
| **The file itself** | S3-compatible object storage (Filebase in production, MinIO locally, local disk only in dev) | `contract-v2.pdf` |
| **The record** | PostgreSQL tables `documents`, `document_types`, `stored_files` | title, project, type, version, uploader, SHA-256 |

The storage layer picks an adapter at runtime (`core/storage/index.ts`):
> put → **S3** if configured, else Vercel Blob if a token exists, else local disk. Reads follow the URL, so old Blob files stay readable.

Since 12 Nov the API **refuses to start in production without S3/Blob**, because local disk on Render is wiped every deploy (security fix #1).

## 🔵 Protections
| Protection | How |
|---|---|
| **Type sniffing** | Reads the first bytes ("magic numbers") — a `.png` must really be a PNG (`uploads/sniff.ts`) |
| **Size limit** | `MAX_UPLOAD_MB` (recommend ~50 on Render) |
| **Integrity check** | SHA-256 fingerprint at upload; re-checked on download (`uploads/integrity`) |
| **Download audit** | Who, which file, which project; *Legal* documents logged as `sensitive_download` for Security Monitoring |
| **Authenticated route** | Files are served only through the API with a token, and only to roles/project members allowed to see that document (fix #14) |
| **Who can write** | Admin edits/deletes docs and types; IT may upload; Site Personnel & Consultant upload; Owner can't create |

## Related flows
- Blueprints: project → architect folder tree, PDF page-1 preview image, DWG accepted.
- Document types are editable (client demo change #1); required types per phase feed gates (Notice of Award, Contract, Notice to Proceed, Certificate of Completion…). → [[Lifecycle Gates]]

Related: [[Security Hardening Nov 2026]] · [[Deployment and Hosting]]
