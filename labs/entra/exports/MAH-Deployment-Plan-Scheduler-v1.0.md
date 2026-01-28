# Jacob Williams Identity Lab — IAM Lab Deployment Plan (Scheduler Format) v1.0
Date: 2026-01-16  
Timezone: America/Los_Angeles  
Scope: Microsoft Entra ID + Hybrid multi-forest Active Directory lab for HIPAA/SOC 2-aligned IAM controls  
Dummy Org: MAH (~1,800) + ATS (~450) + External IDs (~2,000)

---

## How to use this plan in a scheduler
- Each task includes: **Time Block**, **Dependency**, **Output**, and **Validation Evidence**.
- Schedule in 60–120 minute sessions. Stop at validation checkpoints to avoid drift.
- “Evidence” items are what you’ll screenshot/export for compliance-style proof.

---

## Reference Architecture (Target State)
- Forests/Domains:
  - `corp.jpw.lab` (account forest; child domains: `east.corp.jpw.lab`, `west.corp.jpw.lab`)
  - `ats.jpw.lab` (subsidiary forest)
- UPNs:
  - MAH: `@mah.jacobwilliams.cloud`
  - ATS: `@ats.jacobwilliams.cloud`
- Entra tenant: `jacobwilliams.cloud`
- Sync: Microsoft Entra Cloud Sync from `corp` + `ats`
- Governance: CA + PIM + Access Reviews + logging/evidence

---

## Milestones
- M0: Prereqs complete
- M1: Forests/DNS operational
- M2: Trust & hardening complete
- M3: Tenant + domains configured
- M4: Cloud Sync operational
- M5: Conditional Access baseline enforced
- M6: PIM operational
- M7: Test identities + lifecycle flows operating
- M8: Logging/evidence baseline established
- M9: Validation + break-glass verified

---

# PHASE 0 — Prereqs & Naming (M0)
**Goal:** Ensure lab readiness, naming, and foundational governance assumptions.  
**Recommended total:** 2–4 hours

## P0.1 Lab Inventory & Access
- **Time Block:** 60 min
- **Dependency:** None
- **Tasks:**
  - Confirm admin workstation(s) + server hosts (VMs ok)
  - Confirm outbound internet for Entra + endpoints
  - Confirm time sync source (NTP) plan
- **Output:** Lab inventory notes
- **Validation Evidence:** Screenshot of host/VM list + time sync config

## P0.2 Naming & Identity Conventions
- **Time Block:** 60 min
- **Dependency:** P0.1
- **Tasks:**
  - Confirm DNS namespaces: `corp.jpw.lab`, `ats.jpw.lab`
  - Confirm UPN suffixes: `mah.jacobwilliams.cloud`, `ats.jacobwilliams.cloud`
  - Draft OU naming + admin tiering labels
- **Output:** Naming conventions section ready
- **Validation Evidence:** Saved conventions doc/snippet

## P0.3 Security Baselines (Decisions)
- **Time Block:** 60–120 min
- **Dependency:** P0.2
- **Tasks:**
  - Define Tier model (0/1/2) and admin separation
  - Define break-glass requirements (2 accounts; exclusions)
  - Define logging retention targets (e.g., SOC2 evidence)
- **Output:** Baseline decisions list
- **Validation Evidence:** Baseline decision record

**Phase Exit Criteria:** You have a written naming standard + baseline security decisions.

---

# PHASE 1 — Forests & DNS (M1)
**Goal:** Build AD forests/domains and reliable name resolution.  
**Recommended total:** 6–10 hours

## P1.1 Build `corp.jpw.lab` Forest + Child Domains
- **Time Block:** 2–4 hours
- **Dependency:** Phase 0 complete
- **Tasks:**
  - Deploy DCs for root + child domains (east/west)
  - Configure sites/subnets (even if simplified)
- **Output:** Operational forest + replication
- **Validation Evidence:** `dcdiag` clean; replication summary; Sites/Subnets screenshot

## P1.2 Build `ats.jpw.lab` Forest
- **Time Block:** 2–3 hours
- **Dependency:** P1.1
- **Tasks:**
  - Deploy ATS DC(s)
  - Configure DNS forwarders/conditional forwarders
- **Output:** Operational ATS forest + DNS resolution plan
- **Validation Evidence:** Name resolution tests; `dcdiag`

## P1.3 OU Structure + GPO Scaffolding
- **Time Block:** 2–3 hours
- **Dependency:** P1.1 + P1.2
- **Tasks:**
  - Create Tiered OUs (Tier 0/1/2), Servers, Workstations, Users
  - Create baseline GPOs (password policy, audit policy, admin hardening placeholders)
- **Output:** OU/GPO skeleton
- **Validation Evidence:** OU tree export; GPO list screenshot

**Phase Exit Criteria:** Forests are healthy, DNS resolves across planned boundaries, OU/GPO scaffolding exists.

---

# PHASE 2 — Trusts & Security Hardening (M2)
**Goal:** Establish secure inter-forest access patterns and reduce attack paths.  
**Recommended total:** 6–12 hours

## P2.1 Trust Design + Implementation
- **Time Block:** 2–4 hours
- **Dependency:** Phase 1 complete
- **Tasks:**
  - Configure forest trust (as required for lab)
  - Prefer selective authentication where feasible
- **Output:** Working trust + documented settings
- **Validation Evidence:** Trust properties screenshots; auth test from corp→ats

## P2.2 Admin Tiering Enforcement (Minimum Viable)
- **Time Block:** 2–3 hours
- **Dependency:** P2.1
- **Tasks:**
  - Separate admin accounts (Tier 0 vs daily)
  - Block interactive logon of Tier 0 to lower tiers (GPO)
- **Output:** Tiering separation applied
- **Validation Evidence:** GPO settings + test login failure/success matrix

## P2.3 Core Hardening
- **Time Block:** 2–5 hours
- **Dependency:** P2.2
- **Tasks:**
  - Enable enhanced auditing (DCs + member servers baseline)
  - Confirm LDAP signing/channel binding plan (lab-level)
- **Output:** Audit + hardening baseline
- **Validation Evidence:** Audit policy outputs; event logs showing expected events

**Phase Exit Criteria:** Trust works and is constrained; admin tiers are meaningfully separated; auditing baseline is active.

---

# PHASE 3 — Entra Tenant & Domains (M3)
**Goal:** Configure tenant, verify domains, establish admin model and break-glass.  
**Recommended total:** 4–8 hours

## P3.1 Tenant Baseline Configuration
- **Time Block:** 60–120 min
- **Dependency:** Phase 0 complete (can start earlier)
- **Tasks:**
  - Verify tenant settings (company info, locations, admin contacts)
  - Set default settings aligned with least privilege
- **Output:** Tenant baseline configured
- **Validation Evidence:** Tenant settings screenshots

## P3.2 Custom Domains + DNS Verification
- **Time Block:** 60–120 min
- **Dependency:** P3.1
- **Tasks:**
  - Add `mah.jacobwilliams.cloud` and `ats.jacobwilliams.cloud`
  - Verify via DNS TXT records
  - Add UPN suffixes in AD (if not already)
- **Output:** Verified domains + UPN plan
- **Validation Evidence:** Domain verification status screenshots

## P3.3 Emergency Access (Break-Glass)
- **Time Block:** 60–120 min
- **Dependency:** P3.1
- **Tasks:**
  - Create 2 emergency access accounts (cloud-only)
  - Exclude from CA policies (explicitly documented)
  - Store credentials securely + test sign-in
- **Output:** Break-glass accounts ready
- **Validation Evidence:** Account list + sign-in proof + storage attestation note

**Phase Exit Criteria:** Tenant is configured, domains verified, break-glass accounts exist and are tested.

---

# PHASE 4 — Cloud Sync (M4)
**Goal:** Synchronize identities from both forests into Entra with safe scope and attributes.  
**Recommended total:** 6–12 hours

## P4.1 Cloud Sync Agent Prep
- **Time Block:** 60–120 min
- **Dependency:** Phase 1 complete
- **Tasks:**
  - Select staging OU(s) for pilot sync
  - Ensure least-privileged service account approach
- **Output:** Pilot scope defined
- **Validation Evidence:** OU scope doc + account permissions summary

## P4.2 Deploy Cloud Sync for `corp.jpw.lab`
- **Time Block:** 2–4 hours
- **Dependency:** P4.1
- **Tasks:**
  - Install agent, configure connector, set scoping filters
  - Map UPN to `@mah.jacobwilliams.cloud`
- **Output:** Corp identities syncing
- **Validation Evidence:** Sync status dashboard; sample user in Entra

## P4.3 Deploy Cloud Sync for `ats.jpw.lab`
- **Time Block:** 2–4 hours
- **Dependency:** P4.2
- **Tasks:**
  - Install agent, configure connector, set scoping filters
  - Map UPN to `@ats.jacobwilliams.cloud`
- **Output:** ATS identities syncing
- **Validation Evidence:** Sync status dashboard; sample ATS user in Entra

## P4.4 Attribute Hygiene & Collision Controls
- **Time Block:** 2–4 hours
- **Dependency:** P4.2 + P4.3
- **Tasks:**
  - Confirm uniqueness (UPN/proxyAddresses)
  - Validate soft-match rules + duplicate handling approach
- **Output:** Reduced collision risk
- **Validation Evidence:** Documented collision tests + remediation notes

**Phase Exit Criteria:** Both forests sync successfully with controlled scope and validated identity uniqueness.

---

# PHASE 5 — Conditional Access (CA) (M5)
**Goal:** Implement baseline access controls aligned to HIPAA/SOC 2 patterns.  
**Recommended total:** 6–12 hours

## P5.1 Authentication Methods Baseline
- **Time Block:** 60–120 min
- **Dependency:** Phase 3 complete
- **Tasks:**
  - Enable modern MFA methods (Authenticator, FIDO2 if desired)
  - Disable legacy/weak methods where appropriate
- **Output:** Auth methods baseline
- **Validation Evidence:** Auth methods settings screenshots

## P5.2 CA Baseline Policies (Pilot → Enforce)
- **Time Block:** 3–6 hours
- **Dependency:** P5.1 + Phase 4 complete
- **Tasks (minimum set):**
  - Require MFA for admins
  - Block legacy authentication
  - Require MFA for user access (pilot group first)
  - Require compliant device (if device posture included) OR require approved client apps
  - Session controls for sensitive apps (where applicable)
- **Output:** CA policies implemented with safe rollout
- **Validation Evidence:** CA policy exports/screenshots + sign-in logs showing enforcement

## P5.3 Identity Protection Integration (If licensed)
- **Time Block:** 2–4 hours
- **Dependency:** P5.2
- **Tasks:**
  - Configure user risk/sign-in risk policies
  - Set alerting thresholds + responses
- **Output:** Risk-based controls active
- **Validation Evidence:** Policy screenshots + sample detections (if possible)

**Phase Exit Criteria:** Baseline CA is in place, piloted, then enforced with documented exclusions.

---

# PHASE 6 — PIM & Privileged Access (M6)
**Goal:** Establish least privilege with just-in-time elevation and approvals/auditing.  
**Recommended total:** 6–10 hours

## P6.1 Role Model & Groups
- **Time Block:** 2–4 hours
- **Dependency:** Phase 3 complete
- **Tasks:**
  - Define admin roles: Global Admin (min), Privileged Role Admin, Security Admin, Conditional Access Admin, etc.
  - Implement role-assignable groups where appropriate
- **Output:** Privileged access structure
- **Validation Evidence:** Role assignments export + group config screenshots

## P6.2 PIM Configuration
- **Time Block:** 2–4 hours
- **Dependency:** P6.1
- **Tasks:**
  - Configure activation requirements (MFA, justification)
  - Configure approval (where needed)
  - Configure time-bound activation windows
- **Output:** PIM operational
- **Validation Evidence:** PIM settings screenshots + activation audit record

## P6.3 Access Reviews (Minimum Viable)
- **Time Block:** 1–2 hours
- **Dependency:** P6.2
- **Tasks:**
  - Configure quarterly review for privileged groups/roles
- **Output:** Governance cadence in place
- **Validation Evidence:** Access Review config screenshot

**Phase Exit Criteria:** Privileged access is JIT-controlled with auditable activation.

---

# PHASE 7 — Identities & Sample Data (M7)
**Goal:** Create realistic users/groups/apps and demonstrate joiner–mover–leaver flows.  
**Recommended total:** 4–8 hours

## P7.1 Seed Users/Groups in AD + Entra
- **Time Block:** 2–4 hours
- **Dependency:** Phase 4 complete
- **Tasks:**
  - Create sample users across MAH + ATS (departments, locations)
  - Create security groups for app access
- **Output:** Test population ready
- **Validation Evidence:** User list + group membership samples

## P7.2 HR Authoritative Source Simulation
- **Time Block:** 1–2 hours
- **Dependency:** P7.1
- **Tasks:**
  - Build CSV-based HR feed (joiner/mover/leaver)
  - Document mapping assumptions
- **Output:** HR feed artifact
- **Validation Evidence:** CSV + mapping doc snippet

## P7.3 App Integrations (Minimum Set)
- **Time Block:** 1–2 hours
- **Dependency:** P7.1
- **Tasks:**
  - Create 1–2 enterprise apps (SAML/OIDC mock ok)
  - Assign via groups; test CA behavior
- **Output:** App access flows validated
- **Validation Evidence:** App config screenshots + sign-in logs

**Phase Exit Criteria:** You can demonstrate onboarding, access assignment, and controlled sign-in.

---

# PHASE 8 — Logging & Evidence (M8)
**Goal:** Ensure auditability for HIPAA/SOC 2 style evidence collection.  
**Recommended total:** 4–8 hours

## P8.1 Entra Logging Baseline
- **Time Block:** 1–2 hours
- **Dependency:** Phase 3 complete
- **Tasks:**
  - Validate audit logs + sign-in logs visibility
  - Define retention approach (native vs export to SIEM)
- **Output:** Logging plan + baseline captured
- **Validation Evidence:** Log views + retention settings screenshots

## P8.2 Centralize Logs (If available)
- **Time Block:** 2–4 hours
- **Dependency:** P8.1
- **Tasks:**
  - Configure export to Log Analytics/SIEM (lab-appropriate)
  - Confirm queries for CA, PIM, risky sign-ins
- **Output:** Central logging operational
- **Validation Evidence:** Workspace ingestion proof + sample queries

## P8.3 Evidence Pack Structure
- **Time Block:** 60–120 min
- **Dependency:** P8.1
- **Tasks:**
  - Create folder structure for evidence (per phase)
  - Define “evidence checklist” for recurring audits
- **Output:** Evidence pack ready
- **Validation Evidence:** Folder tree + checklist

**Phase Exit Criteria:** You can reliably produce logs and evidence for changes and access events.

---

# PHASE 9 — Validation & Break-Glass (M9)
**Goal:** Prove controls work end-to-end and that emergency access is viable.  
**Recommended total:** 4–8 hours

## P9.1 End-to-End Control Tests
- **Time Block:** 2–4 hours
- **Dependency:** Phases 4–6 complete
- **Tasks:**
  - Test user sign-in with MFA requirement
  - Test admin PIM elevation + CA enforcement
  - Test blocked legacy auth (expected failure)
- **Output:** Test results documented
- **Validation Evidence:** Sign-in logs, PIM audit logs, failure screenshots

## P9.2 Break-Glass Drill
- **Time Block:** 1–2 hours
- **Dependency:** Phase 3 break-glass complete + CA live
- **Tasks:**
  - Confirm break-glass accounts can access admin portal
  - Confirm monitoring/alerting plan for their use
- **Output:** Break-glass verified + monitored
- **Validation Evidence:** Successful sign-in + alert rule evidence (if configured)

## P9.3 Risk Register Snapshot + Next Iteration Plan
- **Time Block:** 60–120 min
- **Dependency:** P9.1 + P9.2
- **Tasks:**
  - Document top risks + mitigations
  - List backlog items (device compliance, resource forest, PAM, etc.)
- **Output:** Risk register + roadmap
- **Validation Evidence:** Risk register v1.0 saved

**Phase Exit Criteria:** You have repeatable test results and a documented security/governance posture.

---

## Scheduler-Friendly Work Packages (Suggested Session Blocks)
Use these as ready-to-schedule sessions (each 60–120 min):
1) P0.1 Lab inventory  
2) P0.2 Naming conventions  
3) P0.3 Security baselines  
4) P1.1 Corp forest build (session A)  
5) P1.1 Corp forest build (session B)  
6) P1.2 ATS forest build  
7) P1.3 OU/GPO scaffolding  
8) P2.1 Trust configuration  
9) P2.2 Tiering enforcement  
10) P2.3 Audit/hardening baseline  
11) P3.1 Tenant baseline  
12) P3.2 Domain verification + UPN  
13) P3.3 Break-glass setup + test  
14) P4.2 Corp Cloud Sync deploy  
15) P4.3 ATS Cloud Sync deploy  
16) P4.4 Attribute hygiene  
17) P5.1 Auth methods baseline  
18) P5.2 CA baseline (pilot)  
19) P5.2 CA baseline (enforce)  
20) P6.1 Role model + groups  
21) P6.2 PIM config + activation test  
22) P6.3 Access Reviews setup  
23) P7.1 Seed users/groups  
24) P7.3 App integration + CA test  
25) P8.1 Logging baseline  
26) P8.3 Evidence pack structure  
27) P9.1 End-to-end tests  
28) P9.2 Break-glass drill  
29) P9.3 Risk register + roadmap

---

# Appendix A — Quick-Start Guide (How to work with this Coach)
## Common commands you can use
- **“show current deployment status”** — I’ll display phase-by-phase status for this session.
- **“Phase X complete”** — I’ll mark it done, summarize validation checks, and give the next phase plan.
- **“reset deployment plan”** — Resets the phase tracking for the current session.
- **“export bundle”** — I’ll generate a Markdown bundle with: deployment plan, CA policies, PIM config, lifecycle flow, compliance mapping, diagrams, and risk register (and increment version numbers).

## How to run each phase effectively
1) Schedule a session for build tasks  
2) End the session with validation tasks + evidence capture  
3) Record exceptions explicitly (why + when you’ll fix)  
4) Only then move to the next phase

## Evidence tips (HIPAA/SOC 2 friendly)
- Capture **before/after** screenshots for changes (CA, PIM, trust settings).
- Save exports (policy JSON where possible) into an “Evidence/Phase-X” folder.
- Keep a simple change log: date, change, approver (even if lab-simulated).

## If you want an even more scheduler-native format
Ask: **“Export this plan as CSV with columns: Phase, TaskID, Task, Duration, Dependencies, Evidence”**  
(I’ll produce a clean CSV you can import into many planning tools.)

# Appendix B - Portfolio GitHub 
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