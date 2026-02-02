# Phase 0 Completion Checklist (M0) — Prereqs & Naming (KVM Host)
**Goal:** Lab readiness, naming standards, and baseline security decisions are documented and evidence-captured.  
**Exit Criteria:** All items marked **Required** are completed and evidence is stored in the evidence pack.

**Evidence folder:** `exports/Evidence-Pack-v1.0/Phase-0/`

> **Sanitization rule (public repo):** No public IPs (v4/v6), MACs, DUID/IAID, router WAN details, secrets, recovery codes, or QR codes. Mask private IPs to `192.168.x.xx`.

---

## P0.1 Lab Inventory & Access (Required)

### Build / Document
- [x] **Lab inventory recorded** in `docs/00-overview/lab-inventory.md`
  - Includes: host OS (Linux Mint), hardware (CPU/RAM), hypervisor (KVM/libvirt + virt-manager)
  - Includes: bridged networking (`br0`) + LAN reachability intent
  - Includes: systems inventory (Planned → Built) with roles and OS versions
- [x] **Admin access method validated** (jumpbox + host management)
  - RDP to `JPW-ADM-01` works from admin workstation
  - Host remote access is restricted appropriately (SSH only recommended; XRDP disabled)
- [x] **IP management approach documented**
  - DHCP scope is broad → use DHCP reservations for host + lab VMs (DCs/jumpbox)
- [x] **Time sync plan documented** (host + Windows guests)

### Validation (Minimum)
- [x] **KVM acceleration validated** on host (`kvm-ok`)
- [x] **libvirt daemon running** (`systemctl status libvirtd`)
- [x] **Bridge active** (`ip -br addr` shows `br0`)
- [x] **Outbound HTTPS validated** from `JPW-ADM-01` (for Entra/Cloud Sync later):
  - `Resolve-DnsName login.microsoftonline.com`
  - `Test-NetConnection login.microsoftonline.com -Port 443`
- [x] **Time sync verified**
  - Host: `timedatectl` shows NTP synced (or equivalent)
  - Windows (JPW-ADM-01): `w32tm /query /status`

### Evidence (Screenshots) — Required
- [x] **P0.1-VM-Inventory.png**
  - virt-manager view showing VM list (at least `JPW-ADM-01`) and power state
- [x] **P0.1-KVM-Ready.png**
  - `kvm-ok` output + `systemctl status libvirtd` (and optionally `ip -br addr | grep br0`)
- [x] **P0.1-Host-UFW-SSH-Restricted.png**
  - `ufw status numbered` showing inbound 22 allowed only from admin workstation IP (/32)
  - If XRDP is enabled temporarily, show 3389 restricted; otherwise note XRDP disabled
- [x] **P0.1-JPW-ADM-01-RDP-Enabled.png**
  - Proof of successful RDP session to `JPW-ADM-01` (sanitized)
- [x] **P0.1-TimeSync-Mint.png**
  - `timedatectl` (or chrony output) showing sync status
- [x] **P0.1-TimeSync-JPW-ADM-01.png**
  - `w32tm /query /status` output
- [ ] (Optional) **P0.1-Connectivity-TestNetConnection.png**
  - `Resolve-DnsName` + `Test-NetConnection` output proof

---

## P0.2 Naming & Identity Conventions (Required)

### Build / Document
- [x] **Naming Standard doc saved** (v1.0+) in `docs/00-overview/`
- [x] Includes **AD DNS namespaces**:
  - `corp.jpw.lab` (root)
  - `east.corp.jpw.lab`, `west.corp.jpw.lab` (children)
  - `ats.jpw.lab` (subsidiary forest)
- [x] Includes **UPN suffixes**:
  - `mah.jacobwilliams.cloud`
  - `ats.jacobwilliams.cloud`
- [x] Includes **reserved prefixes**: `bg-`, `t0-`, `t1-`, `t2-`, `svc-`, `gmsa-`, etc.
- [x] Includes **minimal “Admin OU intent”** section (Draft; refined in Phase 1)

### Validation
- [x] Conventions document reviewed for internal consistency (matches target state)

### Evidence (Screenshots)
- [x] **P0.2-Naming-Conventions-Excerpt.png**
  - snippet showing namespaces + UPNs + tier/admin naming intent

---

## P0.3 Security Baselines (Decisions) (Required)

### Build / Document
- [x] **Security baselines document saved** in `docs/00-overview/security-baselines-decisions.md`
- [x] **Tier model defined** (what belongs to Tier 0/1/2)
- [x] **Admin separation rules defined**
  - Tier 0 admin accounts separate from daily-use accounts
  - Tier 0 logon restrictions documented (implemented in Phase 2)
- [x] **Break-glass requirements defined**
  - 2 cloud-only accounts
  - Exclusions documented (excluded from CA)
  - Storage/process documented (vault + access process)
- [x] **Logging/retention targets defined**
  - Log sources: Entra sign-in/audit + PIM
  - Retention approach: native + evidence exports
  - Evidence pack strategy: phase-based artifacts

### Validation
- [ x Baseline decisions reviewed against HIPAA/SOC2 intent:
  - MFA + least privilege + auditability explicitly covered (at “control intent” level)

### Evidence (Screenshots)
- [x] **P0.3-Tiering-Model-Decision.png**
- [x] **P0.3-BreakGlass-Decision.png**
- [x] **P0.3-Logging-Retention-Decision.png**

---

## Phase 0 Exit Criteria (M0) — Must Be True
- [x] P0.1 complete: lab inventory exists; access validated and restricted; host virtualization and time sync validated
- [x] P0.2 complete: naming standard is written and consistent with target state
- [x] P0.3 complete: baseline security decisions documented (tiering, break-glass, logging)
- [x] Evidence screenshots exist in `exports/Evidence-Pack-v1.0/Phase-0/`
- [x] No secrets/tokens/passwords/public IPs/MACs/DUIDs were committed

---

## Evidence File Naming (Recommended)
- `P0.1-VM-Inventory.png`
- `P0.1-KVM-Ready.png`
- `P0.1-Host-UFW-Restricted.png`
- `P0.1-JPW-ADM-01-RDP-Validated.png`
- `P0.1-TimeSync-Host.png`
- `P0.1-TimeSync-JPW-ADM-01.png`
- `P0.1-Connectivity-TestNetConnection.png` (optional)
- `P0.2-Naming-Conventions-Excerpt.png`
- `P0.3-Tiering-Model-Decision.png`
- `P0.3-BreakGlass-Decision.png`
- `P0.3-Logging-Retention-Decision.png`
