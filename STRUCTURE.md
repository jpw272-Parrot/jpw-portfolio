# Repository Structure & Rules

This repository is intentionally structured for:
- **Portfolio readability** (architect story first)
- **Repeatability** (scripts + validation)
- **Clean diffs** (diagram source vs exports)
- **Compliance friendliness** (sanitized evidence + mapping)

## Folder Layout

mah-entra-hybrid-lab/
├─ README.md
├─ STRUCTURE.md
├─ LICENSE
├─ .gitignore
├─ docs/
│ ├─ 00-overview/
│ ├─ 01-current-state/
│ ├─ 02-target-architecture/
│ ├─ 03-entra-id/
│ ├─ 04-hybrid-ad/
│ ├─ 05-cloud-sync/
│ ├─ 06-conditional-access/
│ ├─ 07-pim-privileged-access/
│ ├─ 08-identity-governance/
│ ├─ 09-logging-evidence/
│ ├─ 10-validation/
│ └─ 11-risk-compliance/
├─ diagrams/
│ ├─ source/
│ │ ├─ mermaid/
│ │ └─ drawio/ (optional)
│ └─ exports/
│ ├─ svg/
│ └─ png/
├─ scripts/
│ ├─ README.md
│ ├─ powershell/
│ │ ├─ entra/
│ │ ├─ ad/
│ │ ├─ logging/
│ │ └─ utilities/
│ ├─ graph/ (optional)
│ └─ templates/
├─ data/
│ ├─ sample-identities/
│ └─ sample-logs/
└─ exports/
├─ MAH-Deployment-Bundle-v1.0.md
└─ Evidence-Pack-v1.0/

## What Goes Where

### `docs/` (Primary narrative)
Design and decision records:
- architecture goals, constraints, assumptions
- target-state design and rationale
- policy definitions (CA/PIM)
- lifecycle and governance
- validation plans and results
- risk register and compliance mappings

**Rule:** Docs should be written so a reviewer can understand the design without running code.

### `diagrams/`
- `diagrams/source/` contains diagram *source* files (Mermaid `.mmd`, Draw.io `.drawio`)
- `diagrams/exports/` contains rendered outputs (`.svg`/`.png`) referenced by docs

**Rule:** Always commit the source + export together (when feasible).

### `scripts/`
Automation and validation tooling.
- Scripts must include **safe-run** notes (permissions, what it changes)
- Scripts should default to **read-only/reporting** unless explicitly marked otherwise

### `data/`
Sanitized sample data for demo purposes only (CSV exports, synthetic identity lists, scrubbed logs).

### `exports/`
“Print-ready” or “share-ready” bundles:
- consolidated Markdown bundles
- evidence packs (sanitized)
- versioned deliverables

## Naming & Versioning

### Folder ordering
Use `00-`, `01-`, etc. to enforce a reading path.

### Export versioning
Use semantic-ish versions:
- `v1.0` initial complete bundle
- `v1.1`, `v1.2` incremental refinements

### File naming
Use hyphenated names:
- `conditional-access-policy-set.md`
- `pim-role-design.md`
- `risk-register.csv`

## Evidence & Sanitization Rules (Non-negotiable)
Do not commit:
- secrets (client secrets, cert private keys, tokens)
- tenant IDs, real domains, real user emails
- raw sign-in logs if they include real UPNs/IPs

Do:
- use synthetic users (e.g. `alex.wu@mahhealth.org`)
- scrub IPs or replace with RFC5737 test blocks (e.g. `203.0.113.0/24`)
- scrub GUIDs if they can identify a tenant

## Git Tip: Empty Folders
Git won’t track empty folders. Place a small `README.md` in each folder you want retained.

