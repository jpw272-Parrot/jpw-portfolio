# Phase 0 Evidence Pack — Auditor View (M0)

**Scope:** Phase 0 (Prereqs & Naming) evidence for Jacob Williams Identity Lab  
**Purpose:** Provide quick, reviewable proof that foundational lab prerequisites, naming standards, and baseline IAM security decisions were established.  
**Evidence Location:** `exports/Evidence-Pack-v1.0/Phase-0/`  
**Date Range Covered:** 2026-01-16 → __________ (fill in)

---

## 1) Lab Inventory, Connectivity, and Time Sync (P0.1)

### Required Evidence
- [ ] **P0.1-VM-Inventory.png**
  - **Must show:** VM/host list including key lab systems (DCs, admin workstation, utility servers if any), OS versions, and power state.
  - **Pass criteria:** Reviewer can identify the lab systems and confirm they exist and are running.

- [ ] **P0.1-TimeSync-w32tm-status-DC1.png**
  - **Must show:** `w32tm /query /status` output from a DC (preferred) or core server, including time source and last sync time.
  - **Pass criteria:** Valid time source and recent successful sync visible.

### Optional (Recommended)
- [ ] **P0.1-Connectivity-TestNetConnection.png**
  - **Must show:** DNS resolution and/or port connectivity test from admin workstation to a DC (e.g., `nslookup` + `Test-NetConnection -Port 53`).
  - **Pass criteria:** Successful resolution/connectivity visible.

**Reviewer Notes:**  
- Findings/Exceptions: ____________________________________________  
- Risk Level (Low/Med/High): _______

---

## 2) Naming & Identity Conventions (P0.2)

### Required Evidence
- [ ] **P0.2-Naming-Conventions-Excerpt.png**
  - **Must show (in the document snippet):**
    - AD DNS namespaces: `corp.jpw.lab`, `east.corp.jpw.lab`, `west.corp.jpw.lab`, `ats.jpw.lab`
    - UPN suffixes: `mah.jacobwilliams.cloud`, `ats.jacobwilliams.cloud`
    - Tiering labels and OU naming conventions (at least the pattern)
  - **Pass criteria:** Namespaces and UPN conventions are clearly defined and consistent with the target state.

**Reviewer Notes:**  
- Findings/Exceptions: ____________________________________________  
- Risk Level (Low/Med/High): _______

---

## 3) Baseline Security Decisions (P0.3)

### Required Evidence
- [ ] **P0.3-Tiering-Model-Decision.png**
  - **Must show:** Tier 0/1/2 definition and admin separation rules (what accounts/systems belong to which tier).
  - **Pass criteria:** Clear tier model with explicit separation expectations.

- [ ] **P0.3-BreakGlass-Decision.png**
  - **Must show:** Requirement for two cloud-only emergency access accounts, exclusion intent, and storage/handling approach (no passwords shown).
  - **Pass criteria:** Break-glass approach is defined and includes operational safeguards.

- [ ] **P0.3-Logging-Retention-Decision.png**
  - **Must show:** What logs will be relied upon (Entra sign-in/audit + PIM), retention approach (native vs export), and evidence pack strategy.
  - **Pass criteria:** Logging and evidence strategy is documented and supports auditability.

**Reviewer Notes:**  
- Findings/Exceptions: ____________________________________________  
- Risk Level (Low/Med/High): _______

---

## Phase 0 Overall Attestation (M0)

### Pass/Fail
- [ ] **PASS** — All required evidence items present and meet pass criteria  
- [ ] **FAIL** — Missing required evidence and/or does not meet pass criteria

### Exceptions / Compensating Controls (if any)
- Exception: _________________________________________________  
- Rationale: _________________________________________________  
- Compensating control: _______________________________________  
- Target remediation date: ____________________________________

### Sign-off (Lab Owner)
- Name: Jacob Williams  
- Date: ____________  
- Signature/Initials: ____________
