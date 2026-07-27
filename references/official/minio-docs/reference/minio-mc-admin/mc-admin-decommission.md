---
title: "mc admin decommission — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-decommission.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-decommission"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin decommission — SILO (MinIO Object Storage)"
---
# mc admin decommission

## Syntax

The [`mc admin decommission`](#command-mc.admin.decommission) command starts the decommissioning process for a
MinIO [[operations/concepts#minio-intro-server-pool|server pools]]. Decommissioning is designed
for removing an older server pool whose hardware is no longer sufficient or
performant compared to the pools in the deployment. MinIO automatically migrates
data from the decommissioned pool to the remaining pools in the deployment based
on the ratio of free space available in each pool.

See [[operations/deployments/baremetal-decommission-server-pool#minio-decommissioning|Decommission Server Pools]] for a complete procedure on
decommissioning a server pool.

Decommissioning is Permanent

Once MinIO begins decommissioning a pool, it marks that pool as permanently
inactive (“draining”). Cancelling or otherwise interrupting the
decommissioning procedure does not restore the pool to an active
state.

Decommissioning is a major administrative operation that requires care
in planning and execution, and is not a trivial or ‘daily’ task.

[MinIO SUBNET](https://min.io/pricing?jmp=docs) users can
[log in](https://subnet.min.io/) and create a new issue related to
decommissioning. Coordination with MinIO Engineering via SUBNET can ensure
successful decommissioning, including performance testing and health
diagnostics.

Community users can seek support on the [MinIO Community Slack](https://slack.min.io). Community Support is best-effort only and has
no SLAs around responsiveness.

```
mc admin [GLOBALFLAGS] decommission start|status|cancel ALIAS TARGET
```

### Parameters

start[](#mc.admin.decommission.start)

Required Starts the decommissioning process for the server pool specified
to [`TARGET`](#mc.admin.decommission.TARGET).

Requires specifying [`TARGET`](#mc.admin.decommission.TARGET)

status[](#mc.admin.decommission.status)

Required Returns the decommissioning status of all server pools on the
specified [`ALIAS`](#mc.admin.decommission.ALIAS):

Active - The pool is active and not scheduled for
decommissioning.

Draining - The pool is currently decommissioning.

Draining (Failed) - The decommissioning process failed and
requires manually restart.

Draining (Cancelled) - The decommissioning process was
manually cancelled.

If the command includes a [`TARGET`](#mc.admin.decommission.TARGET),
the command output includes the rate of data migration if
decommissioning is in progress.

cancel[](#mc.admin.decommission.cancel)

Required Cancels an ongoing decommissioning process on the pool specified
to [`TARGET`](#mc.admin.decommission.TARGET).

Requires specifying [`TARGET`](#mc.admin.decommission.TARGET).

Cancelling a decommissioning process does not return the pool to an active
state. You must eventually complete the decommissioning process and remove
the pool from the deployment. You can resume the process by
running [`mc admin decommission start`](#mc.admin.decommission.start) again against the pool.

ALIAS[](#mc.admin.decommission.ALIAS)

Required The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment on which to start
the decommissioning process.

TARGET[](#mc.admin.decommission.TARGET)

The full description of the server pool on which the command operates. For
example:

```
https://minio-{01...04}.example.net:9000/mnt/disk{1...4}
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

See [[operations/deployments/baremetal-decommission-server-pool#minio-decommissioning|Decommission Server Pools]] for a complete procedure on
decommissioning a server pool.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).