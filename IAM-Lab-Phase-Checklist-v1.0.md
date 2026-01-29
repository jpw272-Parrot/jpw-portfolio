# Jacob Williams Identity Lab — Phase Checklist v1.0
Date created: 2026-01-28  
Scope: Microsoft Entra ID + Hybrid multi-forest Active Directory lab (HIPAA/SOC 2 aligned)  
Target State: `corp.jpw.lab` (+ `east`, `west`) and `ats.jpw.lab` syncing to Entra tenant `jacobwilliams.cloud`

---

## How to use
- Check items as completed: `[ ]` → `[x]`
- Store phase evidence in: `exports/Evidence-Pack-v1.0/Phase-X/`
- Use the “Phase Exit Criteria” section to determine if the phase is truly done.

---

# PHASE 0 — Prereqs & Naming (M0)

## P0.1 Lab Inventory & Access
- [x] Lab inventory recorded in `docs/00-overview/lab-inventory.md`
- [x] Admin access method validated (RDP/SSH) and restricted appropriately
- [x] Time sync plan documented (host + Windows guests)
- [x] Evidence: VM/host inventory screenshot saved
- [x] Evidence: `kvm-ok` + `libvirtd` status screenshot saved
- [x] Evidence: JPW-ADM-01 connectivity proof (sanitized) saved

## P0.2 Naming & Identity Conventions
- [x] Naming Standard doc saved (v1.0+) in `docs/00-overview/`
- [x] Includes forests/domains + tenant + verified UPN suffixes
- [x] Includes reserved prefixes (bg-/t0-/svc-/etc.)
- [x] Includes minimal draft Admin OU intent section (not locked)
- [x] Evidence: naming conventions excerpt screenshot saved

## P0.3 Security Baselines (Decisions)
- [ ] Tier model (0/1/2) documented
- [ ] Break-glass requirements documented (2 accounts, exclusions, storage)
- [ ] Logging/retention targets documented (Entra + PIM + evidence packs)
- [ ] Evidence: tiering decision screenshot saved
- [ ] Evidence: break-glass decision screenshot saved
- [ ] Evidence: logging/retention decision screenshot saved

### Phase 0 Exit Criteria (M0)
- [x] Naming standard is complete and consistent with target state
- [ ] Baseline security decisions documented
- [ ] Evidence pack contains required Phase-0 screenshots (sanitized)
- [ ] No secrets/public IPs/MACs committed to repo

---

# PHASE 1 — Forests & DNS (M1)

## P1.1 Build `corp.jpw.lab` forest + child domains
- [ ] `CORP-DC1` VM created and OS installed
- [ ] Promote `CORP-DC1` to new forest `corp.jpw.lab` + DNS
- [ ] `EAST-DC1` VM created and promoted to `east.corp.jpw.lab` (child)
- [ ] `WEST-DC1` VM created and promoted to `west.corp.jpw.lab` (child)
- [ ] Sites/Subnets created (even minimal)
- [ ] Validation: `dcdiag` clean (root + child DCs)
- [ ] Validation: replication health (`repadmin /replsummary`)
- [ ] Evidence: DC list + roles screenshot
- [ ] Evidence: `dcdiag` and `repadmin` screenshots saved

## P1.2 Build `ats.jpw.lab` forest
- [ ] `ATS-DC1` VM created and OS installed
- [ ] Promote `ATS-DC1` to new forest `ats.jpw.lab` + DNS
- [ ] DNS forwarder plan implemented (initial)
- [ ] Validation: `dcdiag` clean (ATS)
- [ ] Evidence: `dcdiag` screenshot saved
- [ ] Evidence: name resolution tests saved

## P1.3 OU Structure + GPO Scaffolding
- [ ] Tiered OUs created (Tier 0/1/2) + Users/Servers/Workstations
- [ ] Baseline GPOs created (placeholders OK)
- [ ] Evidence: OU tree export/screenshot
- [ ] Evidence: GPO list screenshot

### Phase 1 Exit Criteria (M1)
- [ ] Forests are healthy and DNS works as intended
- [ ] Replication healthy (corp)
- [ ] OU/GPO scaffolding exists in both forests
- [ ] Evidence pack contains Phase-1 required artifacts

---

# PHASE 2 — Trusts & Security Hardening (M2)

## P2.1 Trust Design + Implementation
- [ ] Forest trust configured between corp ↔ ats (document direction/type)
- [ ] Selective authentication configured (where feasible)
- [ ] Validation: auth test corp→ats as designed
- [ ] Evidence: trust properties screenshots

## P2.2 Admin Tiering Enforcement (MVP)
- [ ] Separate admin accounts created (Tier 0 vs daily)
- [ ] GPO: block interactive logon for Tier 0 to lower tiers
- [ ] Validation: login success/fail matrix documented
- [ ] Evidence: GPO setting screenshots + test proof

## P2.3 Core Hardening
- [ ] Enhanced auditing baseline enabled
- [ ] LDAP signing/channel binding plan documented (lab-level)
- [ ] Validation: audit events observed as expected
- [ ] Evidence: audit policy outputs + event log samples

### Phase 2 Exit Criteria (M2)
- [ ] Trust works and is constrained
- [ ] Admin tiers are meaningfully separated
- [ ] Auditing baseline active and producing logs

---

# PHASE 3 — Entra Tenant & Domains (M3)

## P3.1 Tenant Baseline Configuration
- [ ] Tenant baseline settings reviewed and configured (admin contacts/locations)
- [ ] Admin model baseline documented
- [ ] Evidence: tenant settings screenshots

## P3.2 Custom Domains + DNS Verification
- [ ] `mah.jacobwilliams.cloud` added + verified in Entra
- [ ] `ats.jacobwilliams.cloud` added + verified in Entra
- [ ] AD UPN suffixes added in both forests
- [ ] Evidence: domain verification screenshots

## P3.3 Emergency Access (Break-Glass)
- [ ] Two cloud-only emergency access accounts created
- [ ] Exclusions from CA explicitly documented
- [ ] Sign-in tested and documented
- [ ] Evidence: sign-in proof + storage attestation note (no secrets)

### Phase 3 Exit Criteria (M3)
- [ ] Domains verified and ready for UPN usage
- [ ] Break-glass accounts exist and tested
- [ ] Tenant baseline documented with evidence

---

# PHASE 4 — Cloud Sync (M4)

## P4.1 Cloud Sync Agent Prep
- [ ] Staging OUs defined (pilot scope)
- [ ] Service account approach documented (least privilege)
- [ ] Evidence: OU scope + permissions summary

## P4.2 Deploy Cloud Sync for `corp.jpw.lab`
- [ ] Agent installed and connector configured
- [ ] Scoping filters configured (pilot OU)
- [ ] UPN mapped to `@mah.jacobwilliams.cloud`
- [ ] Evidence: sync status + sample user in Entra

## P4.3 Deploy Cloud Sync for `ats.jpw.lab`
- [ ] Agent installed and connector configured
- [ ] Scoping filters configured (pilot OU)
- [ ] UPN mapped to `@ats.jacobwilliams.cloud`
- [ ] Evidence: sync status + sample ATS user in Entra

## P4.4 Attribute Hygiene & Collision Controls
- [ ] Uniqueness confirmed (UPN/proxyAddresses)
- [ ] Soft-match/collision handling tested + documented
- [ ] Evidence: collision tests + remediation notes

### Phase 4 Exit Criteria (M4)
- [ ] Both forests syncing successfully with controlled scope
- [ ] Identity uniqueness validated, collision plan documented

---

# PHASE 5 — Conditional Access (M5)

## P5.1 Authentication Methods Baseline
- [ ] Authenticator enabled; weak methods restricted/disabled as appropriate
- [ ] Evidence: auth methods settings screenshots

## P5.2 CA Baseline Policies (Pilot → Enforce)
- [ ] Require MFA for admins
- [ ] Block legacy authentication
- [ ] Require MFA for users (pilot group → enforce)
- [ ] Require compliant device OR approved client apps (document choice)
- [ ] Session controls for sensitive apps (where applicable)
- [ ] Evidence: CA policy exports/screenshots + sign-in logs

## P5.3 Identity Protection (If licensed)
- [ ] User risk policy configured
- [ ] Sign-in risk policy configured
- [ ] Alerting/response documented
- [ ] Evidence: policy screenshots + sample events (if possible)

### Phase 5 Exit Criteria (M5)
- [ ] Baseline CA implemented with safe rollout + exclusions documented
- [ ] Sign-in logs demonstrate enforcement

---

# PHASE 6 — PIM & Privileged Access (M6)

## P6.1 Role Model & Groups
- [ ] Admin roles defined (least privilege)
- [ ] Role-assignable groups implemented where appropriate
- [ ] Evidence: role assignments export + group screenshots

## P6.2 PIM Configuration
- [ ] MFA + justification required for activation
- [ ] Approval configured (where needed)
- [ ] Time-bound activation configured
- [ ] Evidence: activation audit record + settings screenshots

## P6.3 Access Reviews (MVP)
- [ ] Quarterly review configured for privileged roles/groups
- [ ] Evidence: access review config screenshot

### Phase 6 Exit Criteria (M6)
- [ ] JIT privilege operational with auditable activation
- [ ] Reviews scheduled and evidenced

---

# PHASE 7 — Identities & Sample Data (M7)

## P7.1 Seed Users/Groups
- [ ] Sample users created across MAH + ATS
- [ ] Security groups created for app access
- [ ] Evidence: user list + group membership samples

## P7.2 HR Authoritative Source Simulation
- [ ] CSV-based HR feed created (joiner/mover/leaver)
- [ ] Mapping assumptions documented
- [ ] Evidence: CSV + mapping snippet

## P7.3 App Integrations (Minimum Set)
- [ ] 1–2 enterprise apps created (SAML/OIDC mock ok)
- [ ] Group assignments configured
- [ ] CA behavior tested
- [ ] Evidence: app config screenshots + sign-in logs

### Phase 7 Exit Criteria (M7)
- [ ] Onboarding + access assignment + controlled sign-in demonstrated end-to-end

---

# PHASE 8 — Logging & Evidence (M8)

## P8.1 Entra Logging Baseline
- [ ] Audit + sign-in logs visibility confirmed
- [ ] Retention approach documented (native vs export)
- [ ] Evidence: log views + retention screenshots

## P8.2 Centralize Logs (If available)
- [ ] Export to Log Analytics/SIEM configured (lab-appropriate)
- [ ] Queries confirmed (CA, PIM, risky sign-ins)
- [ ] Evidence: ingestion proof + sample queries

## P8.3 Evidence Pack Structure
- [ ] Evidence folders created per phase
- [ ] Evidence checklist documented
- [ ] Evidence: folder tree + checklist

### Phase 8 Exit Criteria (M8)
- [ ] Repeatable evidence collection is possible for IAM changes and access events

---

# PHASE 9 — Validation & Break-Glass (M9)

## P9.1 End-to-End Control Tests
- [ ] User sign-in with MFA requirement tested
- [ ] Admin PIM elevation + CA enforcement tested
- [ ] Legacy auth blocked test executed
- [ ] Evidence: sign-in logs + PIM logs + failure proof

## P9.2 Break-Glass Drill
- [ ] Break-glass access tested with CA live
- [ ] Monitoring/alerting plan documented and (if possible) implemented
- [ ] Evidence: sign-in proof + alert evidence

## P9.3 Risk Register Snapshot + Roadmap
- [ ] Risk register updated (top risks + mitigations)
- [ ] Backlog/roadmap documented
- [ ] Evidence: risk register v1.0 saved

### Phase 9 Exit Criteria (M9)
- [ ] Controls verified end-to-end; emergency access viable; roadmap documented

---

## Current Status Snapshot
- Phase 0: [ ] Not Started  [x] In Progress  [ ] Complete
- Phase 1: [x] Not Started  [ ] In Progress  [ ] Complete
- Phase 2: [x] Not Started  [ ] In Progress  [ ] Complete
- Phase 3: [x] Not Started  [ ] In Progress  [ ] Complete
- Phase 4: [x] Not Started  [ ] In Progress  [ ] Complete
- Phase 5: [x] Not Started  [ ] In Progress  [ ] Complete
- Phase 6: [x] Not Started  [ ] In Progress  [ ] Complete
- Phase 7: [x] Not Started  [ ] In Progress  [ ] Complete
- Phase 8: [x] Not Started  [ ] In Progress  [ ] Complete
- Phase 9: [x] Not Started  [ ] In Progress  [ ] Complete
