# Phase 0 Completion Checklist (M0) — Prereqs & Naming

**Goal:** Lab readiness, naming standards, and baseline security decisions are documented and evidence-captured.  
**Exit Criteria:** All items marked **Required** are completed and evidence is stored in the evidence pack.

**Evidence folder:** `exports/Evidence-Pack-v1.0/Phase-0/`

---

## P0.1 Lab Inventory & Access (Required)

### Build / Document
- [ ] **Lab inventory recorded** (hosts, VMs, roles)
  - Includes: DCs, admin workstation, any utility servers
  - Includes: OS version and IP (private IPs OK)
- [ ] **Outbound internet confirmed** for admin workstation + at least one server
- [ ] **Time sync plan documented** (NTP source, domain hierarchy)

### Validation
- [ ] **DNS + connectivity sanity check** from admin workstation:
  - [ ] `nslookup` to a DC name (or planned DC name)
  - [ ] `Test-NetConnection <dc> -Port 53` (or equivalent)
- [ ] **Time sync verified** on at least one Windows server (DC preferred):
  - [ ] `w32tm /query /status` shows a valid source and recent sync

### Evidence (Screenshots)
- [ ] **P0.1-VM-Inventory.png** — VM/host list showing names + power state + OS
- [ ] **P0.1-TimeSync-w32tm-status-DC1.png** — `w32tm /query /status` output
- [ ] (Optional) **P0.1-Connectivity-TestNetConnection.png** — DNS/port test proof

---

## P0.2 Naming & Identity Conventions (Required)

### Build / Document
- [X] **AD DNS namespaces confirmed**:
  - `corp.jpw.lab` (root)
  - `east.corp.jpw.lab`, `west.corp.jpw.lab` (children)
  - `ats.jpw.lab` (subsidiary forest)
- [X] **UPN suffixes confirmed**:
  - `mah.jacobwilliams.cloud`
  - `ats.jacobwilliams.cloud`
- [X] **OU naming + tiering labels drafted**
  - Tier model labels (Tier 0 / Tier 1 / Tier 2)
  - Admin account naming pattern (example documented)

### Validation
- [X] **Conventions document reviewed for consistency** (names match target state section)

### Evidence (Screenshots)
- [X] **P0.2-Naming-Conventions-Excerpt.png** — doc snippet showing namespaces + UPNs + tier labels

---

## P0.3 Security Baselines (Decisions) (Required)

### Build / Document
- [ ] **Tier model defined** (what belongs to Tier 0/1/2)
- [ ] **Admin separation rules defined**
  - Tier 0 admin accounts separate from daily-use accounts
  - Tier 0 logon restrictions (planned)
- [ ] **Break-glass requirements defined**
  - 2 cloud-only accounts
  - Exclusions documented (will be excluded from CA)
  - Storage approach documented (password manager/vault + access process)
- [ ] **Logging/retention targets defined**
  - What logs matter (Entra sign-in/audit, PIM)
  - Retention approach (native vs export)
  - Evidence pack strategy (what you will capture per phase)

### Validation
- [ ] **Baseline decisions reviewed** against HIPAA/SOC2 intent:
  - MFA + least privilege + auditability explicitly covered

### Evidence (Screenshots)
- [ ] **P0.3-Tiering-Model-Decision.png**
- [ ] **P0.3-BreakGlass-Decision.png**
- [ ] **P0.3-Logging-Retention-Decision.png**

---

## Phase 0 Exit Criteria (M0) — Must Be True
- [ ] Naming standard is written and consistent with target state (`corp.jpw.lab`, `ats.jpw.lab`, UPN suffixes)
- [ ] Baseline security decisions are documented (tiering, break-glass, logging)
- [ ] Evidence screenshots exist in `exports/Evidence-Pack-v1.0/Phase-0/`
- [ ] No secrets/tokens/passwords were captured in screenshots or committed to Git

---

## Evidence File Naming (Recommended)
- `P0.1-VM-Inventory.png`
- `P0.1-TimeSync-w32tm-status-DC1.png`
- `P0.1-Connectivity-TestNetConnection.png` (optional)
- `P0.2-Naming-Conventions-Excerpt.png`
- `P0.3-Tiering-Model-Decision.png`
- `P0.3-BreakGlass-Decision.png`
- `P0.3-Logging-Retention-Decision.png`
