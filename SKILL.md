---
name: security-rhapsody
description: >-
  Check and fix security vulnerabilities on a specified list of devices (local machine, servers,
  dev boxes, CI runners, embedded/vendor BSP devices). Each batch of vulnerabilities lives in its
  own file under batches/ (for example batches/2026-10.md).
---
# Security remediation on specified devices

## Input
- The batch file(s) to apply, e.g. `batches/2026-10.md`.
- A list of devices. For each device give its access method (local shell, SSH host, or other
  remote handle) and its type: server/desktop, CI runner, or vendor BSP device.

## Procedure
1. Work through the devices one at a time. For each item, consult its source for current affected
   versions, maintenance-branch fixes and distribution backports, then check the actual installed
   package and enabled feature. Record the advisory and package revision used for the decision.
2. Record which environments, mounts, containers and runtime entry points were checked. An
   inaccessible or unexamined target is "Unable to confirm", not "Not affected". For compromise
   incidents, assess installation and execution history even when current indicators are absent.
3. Fix confirmed affected targets within the rules below. Report "Mitigated" only when the item's
   batch section explicitly allows that status for a verified mitigation; give evidence and upgrade
   advice, and do not repeat remediation solely because the package version remains affected.
   Interim workarounds a section lists without naming "Mitigated" (for example disabling HTTP/3 or
   capping header sizes) do not qualify: keep the target "Affected" and record the workaround.
   When a fix requires repository changes, leave those changes to the owner and report
   "Needs owner fix" with the project path, affected dependency, proposed version and required
   manifest/lockfile and deployment changes.
4. Record the version before and after every change. Mark "Fixed" only after checking the actual
   runtime or trust behavior after the change; a package update alone does not prove that a
   running process, container or bundled trust store uses the fix.

## Rules
- Only touch the specified devices. Git repositories are read-only: inspect source and lockfiles,
  but leave manifests, lockfiles and repository-local installations unchanged. Do not commit,
  push, or send any messages.
- Before upgrading a kernel, rebooting a host, or stopping a production service, list the impact
  and wait for the user's confirmation.
- If a device is unreachable or permissions are insufficient, mark the item "Unable to confirm",
  state the reason, and do not try to force access another way.
- On vendor BSP devices, do not change the kernel or system-wide security settings (such as user
  namespaces); report the version and mark "Needs vendor patch".
- Record credential types and locations for rotation, without copying secret values into evidence.

## Output
One table per device: item, status, evidence and coverage gaps, version before/after, what the user
still needs to do. Use Affected / Mitigated / Fixed / Not affected / Unable to confirm /
Needs owner fix / Needs vendor patch. Keep unresolved coverage gaps visible even when another
target for the same item was fixed. Finish with a separate list of credentials to rotate and items
that need an owner or vendor.

## Adding a batch
Create `batches/YYYY-MM.md` using the same layout as the existing batches: a numbered section per
issue with affected and fixed versions, a source link, check commands, and fix steps.
