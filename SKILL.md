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
1. Work through the devices one at a time. For each device, run every item in the batch file:
   check first, then fix.
2. Report each item as one of: Affected / Fixed / Not affected / Unable to confirm. Attach
   evidence: command output, paths, version numbers.
3. Record the version before and after every change.

## Rules
- Only touch the specified devices. Do not change Git repositories, push, or send any messages.
- Before upgrading a kernel, rebooting a host, or stopping a production service, list the impact
  and wait for the user's confirmation.
- If a device is unreachable or permissions are insufficient, mark the item "Unable to confirm",
  state the reason, and do not try to force access another way.
- On vendor BSP devices, do not change the kernel or system-wide security settings (such as user
  namespaces); report the version and mark "Needs vendor patch".

## Output
One table per device: item, status, evidence, version before/after, what the user still needs to
do. Finish with a separate list of credentials to rotate and items that need the vendor.

## Adding a batch
Create `batches/YYYY-MM.md` using the same layout as the existing batches: a numbered section per
issue with affected and fixed versions, a source link, check commands, and fix steps.
