# MAH Entra + Hybrid AD End-to-End Lab (Portfolio)

This repository is a single, end-to-end Microsoft Entra ID + Hybrid Active Directory lab, designed as a portfolio-quality case study. It demonstrates architecture decisions, security controls, governance, validation, and evidence collection patterns aligned to HIPAA and SOC 2 expectations.

## What’s Inside
- **Architecture & design docs**: target-state identity architecture with clear decisions and tradeoffs
- **Controls**: Conditional Access, MFA/authentication methods, PIM, identity governance, break-glass
- **Hybrid**: multi-forest considerations, Cloud Sync patterns, UPN strategy, admin tiering
- **Evidence & validation**: repeatable checks and exported artifacts
- **Automation**: PowerShell scripts for validation and evidence collection

## Quick Start (How to Navigate)
Start here:
1. `docs/00-overview/` – scope, assumptions, success criteria, repo structure
2. `docs/02-target-architecture/` – diagrams + target state narrative
3. `docs/06-conditional-access/` and `docs/07-pim-privileged-access/` – core security controls
4. `docs/09-logging-evidence/` and `docs/10-validation/` – proof and test plan
5. `scripts/` – automation used for reporting, validation, and evidence export

## Key Artifacts
- **Target Architecture**: `docs/02-target-architecture/`
- **Conditional Access Policy Set**: `docs/06-conditional-access/`
- **PIM & Admin Model**: `docs/07-pim-privileged-access/`
- **Risk Register + Compliance Mapping**: `docs/11-risk-compliance/`
- **Export bundles** (print-ready): `exports/`

## Repo Conventions
- Design content is ordered using numbered folders (e.g., `06-conditional-access`).
- Diagrams are stored as **source** + **exported** images to keep diffs meaningful.
- Scripts are grouped by domain (Entra, AD, logging) and include safe-run guidance.

See `STRUCTURE.md` for details.

## Safety / Sanitization
This repo uses **sanitized** data only. No secrets, tokens, or tenant-identifying details should be committed. Evidence packs must be scrubbed before committing.

## License
See `LICENSE`.
