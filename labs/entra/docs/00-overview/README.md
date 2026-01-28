# Phase 0 — Prereqs & Naming (M0)

This folder contains the **Phase 0** foundation documents for the Jacob Williams Identity Lab (Entra ID + Hybrid AD).  
Phase 0 is where the lab’s **naming**, **assumptions**, and **security baseline decisions** are defined so later phases stay consistent and auditable.

## Contents (Phase 0 Outputs)

| Task | Document | Purpose |
|---|---|---|
| P0.1 Lab Inventory & Access | `lab-inventory.md` | Host/VM inventory, connectivity notes, and time sync plan |
| P0.2 Naming & Identity Conventions | `naming-identity-conventions.md` | AD DNS namespaces, UPN suffixes, OU naming, admin tier labels |
| P0.3 Security Baselines (Decisions) | `security-baselines-decisions.md` | Tier model, break-glass requirements, logging/retention targets |

## Evidence Location

Phase evidence (screenshots/exports) lives in the evidence pack:

`/exports/Evidence-Pack-v1.0/Phase-0/`

### Evidence Checklist (Phase 0)

**P0.1 — Lab Inventory & Access**
- [ ] Host/VM list screenshot  
- [ ] Time sync (NTP) configuration screenshot  

**P0.2 — Naming & Identity Conventions**
- [x] Saved naming conventions snippet (doc excerpt or screenshot)  
- [x] Proof of intended namespaces/UPNs (notes or screenshot)  

**P0.3 — Security Baselines**
- [ ] Baseline decision record saved (doc excerpt or screenshot)  

## Links to Evidence (fill in as you capture)

- P0.1:
  - `../../exports/Evidence-Pack-v1.0/Phase-0/P0.1-host-vm-list.png`
  - `../../exports/Evidence-Pack-v1.0/Phase-0/P0.1-time-sync.png`
- P0.2:
  - `../../exports/Evidence-Pack-v1.0/Phase-0/P0.2-naming-conventions.png`
- P0.3:
  - `../../exports/Evidence-Pack-v1.0/Phase-0/P0.3-security-baselines.png`

## Phase Exit Criteria (M0)

- [ ] Written naming standard exists and matches the target state (`corp.jpw.lab`, `ats.jpw.lab`, UPNs `@mah.jacobwilliams.cloud`, `@ats.jacobwilliams.cloud`)
- [ ] Baseline security decisions are documented (tiering, break-glass, logging targets)
- [ ] Minimum evidence captured and stored in the Phase-0 evidence folder
