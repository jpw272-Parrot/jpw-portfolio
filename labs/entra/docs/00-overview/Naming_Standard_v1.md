# JPW Identity Lab — Naming Standard (Phase 0)

## Document control
| Field | Value |
|---|---|
| Version | v1.0 |
| Owner | Jacob Williams |
| Approved by | Jacob Williams |
| Effective date | 2026-01-26 |
| Last review | 2026-01-26 |

## Purpose
Define consistent naming and namespace rules for identities, groups, devices, and service accounts to support hybrid Entra ID + AD, and to reduce operational and audit risk.

## Scope
- **Forests/domains:** `corp.jpw.lab` (Child Domains: `east.corp.jpw.lab`, `west.corp.jpw.lab`), `ats.jpw.lab` (subsidiary forest)
- **Entra tenant:** `jacobwilliams.cloud`
- **UPN namespaces:** `@mah.jacobwilliams.cloud` (MAH primary), `@ats.jacobwilliams.cloud` (ATS subsidiary)  
  - `mah.jacobwilliams.cloud` and `ats.jacobwilliams.cloud` are verified subdomains in the `jacobwilliams.cloud` tenant and are used for portfolio segmentation.
- **Exclusions:**
  - Break-glass accounts: `bg-*` (cloud-only; excluded from normal lifecycle/dynamic groups)
  - Privileged admin accounts: `t0-*` / `t1-*` / `t2-*` (separate identities; excluded from broad dynamic membership)
  - Service accounts: `svc-*`, `gmsa-*` (not synced unless explicitly required)
  - Built-in/system objects and default containers (e.g., `Builtin`, `ForeignSecurityPrincipals`)
  - Pilot/test OUs not intended for sync (to be defined in Phase 4 OU filtering)
- **Reserved prefixes:** `bg-`, `t0-`, `t1-`, `t2-`, `svc-`, `gmsa-`, `mi-`, `spn-` (not allowed for standard accounts)

---

## Namespace decisions

### AD internal DNS
| Item | Value | Notes |
|---|---|---|
| Account forest | `corp.jpw.lab` |  |
| Child domain(s) | `east.corp.jpw.lab`, `west.corp.jpw.lab` |  |
| Subsidiary forest | `ats.jpw.lab` |  |
| Resource forest (if any) | `res.jpw.lab` |  |

### Public UPN suffixes
| Org/Population | UPN suffix | Verified in Entra? (Y/N) | Notes |
|---|---|---|---|
| Primary | `@mah.jacobwilliams.cloud` | Y |  |
| Subsidiary | `@ats.jacobwilliams.cloud` | Y |  |
| Contractors (if any) | `@mah.jacobwilliams.cloud` / `@ats.jacobwilliams.cloud` | Y | Contractors are created as standard users in the appropriate org domain and tagged `employeeType=Contractor` (or `extensionAttributeX=Contractor`). |

---

## Identity naming

### User naming
| Attribute | Rule | Example | Collision handling |
|---|---|---|---|
| UPN | UPN must use routable domains only. Allowed chars: `a-z`, `0-9`, `.`. Normalize: lowercase UPN; remove apostrophes/diacritics; remove hyphens from input name (hyphens removed to reduce length; may increase collisions; resolved via numeric suffix). | `firstname.lastname@mah.jacobwilliams.cloud` (MAH) / `firstname.lastname@ats.jacobwilliams.cloud` (ATS) | `firstname.lastname2@domain` |
| sAMAccountName | `<firstInitial><lastname>` truncated to 20 characters | `jwilliams` | add digit suffix: `jwilliams2` |
| Display name | Full name first name and last name, capitalized, unless there is a preferred name, which will replace the first name. PreferredName is authoritative if present. Format suffixes (Jr., III) as applicable. | `First Name Last Name` | `Preferred Name Last Name` |

> **Note:** UPN and sAMAccountName are not reused for other individuals (ever).

### Admin identity separation
- **Admin account prefix/suffix:** depends on the tier of their account access:  
  - `t0-` for Global Admin  
  - `t1-` for Domain Admin  
  - `t2-` for Server Admin  
  Tier labels are portfolio shorthand; actual tiering scope is defined in the Tiering Standard (future).  
  - Example: `t0-jacob.williams`
- **Admin accounts MUST be separate from daily user accounts:** Y
- **Admin accounts have mailbox/Teams enabled:** N (Recommended: N)

### Service accounts
| Type | Rule | Example | Credential standard |
|---|---|---|---|
| gMSA | `gmsa-<app>-<tier>-<env>` (no shared interactive logon; one per app/tier; scoped to specific hosts or gMSA group) | `gmsa-ehr-web-prod` | Preferred for Windows services/IIS/SQL Agent where supported. Managed password rotation by AD. Set `PrincipalsAllowedToRetrieveManagedPassword` to a least-privileged security group (e.g., `GG-JPW-gmsa-ehr-web-prod`). Deny interactive logon via GPO. Monitor service logons (4624 type 5). |
| Traditional svc acct | `svc-<app>-<tier>-<env>` (last resort only; no mailbox; no interactive sign-in; one per app) | `svc-ehr-api-prod` | 40+ char random secret stored in vault; rotate >=90 days (or per policy) and on staff change/incident. Enforce “Deny log on locally/RDP,” “Account is sensitive and cannot be delegated” if applicable, limit to required SPNs only, and restrict to required hosts (Log On To / local security policy where feasible). |
| Cloud workload identity (SPN/MI) | Prefer Managed Identity: `mi-<app>-<env>`; otherwise SPN: `spn-<app>-<env>` with owners + expiry | `mi-telemetry-prod` / `spn-jpwportal-prod` | Prefer Managed Identity (no secrets). For SPNs, avoid client secrets; use cert-based auth where possible, or secrets with <= 6–12 month max lifetime + automated rotation. Enforce least-privileged API permissions; require 2 owners; tag with data classification; log to Entra audit; restrict who can create app registrations. |

> **Service account governance**
> - Each service account must have: **OwnerPrimary**, **System/App**, **Environment**, **RotationCadence**, **DecommissionPlan** recorded (Description field or vault record).
> - Service accounts must be disabled and removed from groups within **3 days** of app decommission.
> - Service accounts must **not** be members of Domain Admins / Enterprise Admins (or equivalent privileged groups).
> - `<env>` values: **DEV / TST / STG / PROD** (uppercase)

---

## Group naming

### Entra security groups
- **Pattern:** `SG-<Scope>-<Resource>-<Role>-<Env>`
- **Required metadata (stored in Group Description):**
  - `OwnerPrimary=<UPN or group>`
  - `OwnerSecondary=<UPN or group>`
  - `TicketOrRFC=<id>`
  - `DataClass=<Public|Internal|Confidential|PHI>`
  - `ReviewCadence=<Quarterly|Semiannual>`
  - `Source=<Manual|Dynamic|Provisioned>`
  - `Lifecycle=<Permanent|TimeBound>`
  - Description must include: **purpose + resource + who should be a member**
- **Example description header:**
  - OwnerPrimary: <UPN>
  - OwnerSecondary: <UPN>
  - DataClass: Internal
  - ReviewCadence: Quarterly
  - TicketOrRFC: LAB-001
- **Owner requirement:** each group must have **two owners** (OwnerPrimary/OwnerSecondary)

### Dynamic groups
- **Pattern:** `DG-<Scope>-<Purpose>-<Env>`
- **Attribute sources:**
- Primary: `user.department`, `user.jobTitle`, `user.companyName`, `user.accountEnabled`, `user.userType`
- On-prem AD attributes: `department`, `company`, `employeeId`, `extensionAttribute1-15`, `physicalDeliveryOfficeName`
- Devices: `device.deviceOSType`, `device.deviceTrustType`, `device.managementType`
- Prefer `employeeId` / `extensionAttribute*` over `jobTitle`
- Exclude `bg-*` and `t0-*` / `t1-*` / `t2-*` accounts
- Record the membership rule text in **Description** and in the evidence binder

### AD groups (optional — AGDLP)
- Global groups: `GG-...`
- Domain local groups: `DL-...`
- Nesting standard: **Accounts → Global groups → Domain Local groups → Permissions**

---

## Device naming
| Device type | Rule | Example |
|---|---|---|
| Workstations | `JPW-<Dept>-PC-<##>` or `JPW-<Dept>-MAC-<##>`; maximum length 20 characters. If the limit is exceeded, use abbreviations (e.g., `MKTG`, `ENG`). | `JPW-MKTG-PC-01` |
| Servers | `JPW-<Service>-<Env>-<##>`; maximum length 20 characters. If the limit is exceeded, use abbreviations. Acceptable env abbreviations: `DEV/TST/STG/PROD`. | `JPW-JENKINS-PROD-01` |

---

## Prohibited patterns
- Using internal suffixes (e.g., `.local`, `.lab`) as UPNs
- Reusing sAMAccountName/UPN for different people
- Shared admin accounts

---

## Change control
- Proposed by: Jacob Williams
- Reviewed by: ChatGPT Entra ID Coach
- Approved by: Jacob Williams
- Notes: ______________________________________
