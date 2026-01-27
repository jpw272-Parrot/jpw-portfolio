# Scripts (Automation & Evidence)

This folder contains scripts used to validate configuration and export evidence for the lab.

## Structure
- `powershell/entra/` – Entra ID reporting/validation (CA, roles, auth methods)
- `powershell/ad/` – AD/Hybrid checks (OU/GPO conventions, tiering checks, DC health)
- `powershell/logging/` – log exports, evidence pack collection
- `powershell/utilities/` – shared helper functions/modules
- `templates/` – reusable JSON/CSV templates (e.g., CA policy definitions, sample identity CSV)

## Safety Model (Default Read-Only)
**Default rule:** scripts should be **read-only** (report/validate/export).
If a script makes changes:
- it must be explicitly named and labeled (e.g., `Set-...` or `New-...`)
- include `-WhatIf` support when practical
- include a “ROLLBACK” section in the header

## Standard Script Header (Recommended)
Every script should start with:
- purpose
- prerequisites (modules, permissions)
- inputs/parameters
- outputs (files, console)
- side effects (if any)
- example usage

## Example Folder Conventions
scripts/powershell/entra/
Get-ConditionalAccessBaselineReport.ps1
Get-PIMRoleAssignmentsReport.ps1
scripts/powershell/logging/
Export-EntraAuditLogs.ps1
Export-EntraSignInLogs.ps1
scripts/powershell/utilities/
Common.psm1

## Evidence Output Location
Scripts that export files should write to:
- `exports/Evidence-Pack-vX.Y/` (preferred), or
- a local temp folder that is ignored by git and then copied in after sanitization.

## Sanitization Guidance
Before committing outputs:
- replace real UPNs with synthetic
- remove tenant IDs and app IDs where they uniquely identify a tenant
- scrub IP addresses unless they are test ranges (RFC5737)
- remove device IDs if tied to real hardware

## Suggested PowerShell Modules
Depending on script needs:
- `Microsoft.Graph` (Graph PowerShell SDK)
- `Az.Accounts` (only if needed)
- `ActiveDirectory` RSAT module (for AD checks)

## Running Scripts
Prefer:
- scoped least-privileged accounts
- dedicated “lab tenant” environments
- explicit scopes in Graph auth
