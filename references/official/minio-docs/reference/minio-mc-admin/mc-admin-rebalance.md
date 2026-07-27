---
title: "mc admin rebalance — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-rebalance.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-rebalance"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin rebalance — SILO (MinIO Object Storage)"
---
# mc admin rebalance

## Permission

This command requires that the user performing it have the [[administration/identity-access-management/policy-based-access-control#policy-action.admin-Rebalance|`admin:Rebalance`]] [[administration/identity-access-management/policy-based-access-control#minio-policy|policy action]] for the deployment.

## Description

The [`mc admin rebalance`](#command-mc.admin.rebalance) command allows starts, monitors, or stops a rebalancing operation on a MinIO deployment.
Rebalancing redistributes objects across all pools in the deployment.

MinIO does not automatically rebalance objects when adding a new server pool.
Instead, MinIO [[operations/deployments/baremetal-expand-minio-deployment#minio-writing-files|writes new objects]] to the pool with relatively more free space compared to the other available pools on the deployment.
Triggering a manual rebalancing procedure prompts MinIO to scan the entire deployment and move objects as necessary to achieve a similar available free space across all pools.

This is an expensive and time consuming operation.
Consider only running a rebalance procedure during light or no use of the deployment.
If write operations do occur during a rebalance operation, they process in parallel and write to a pool not actively in rebalancing.

You can stop a rebalance and start it again later as needed.

Follow the progress of an ongoing rebalance operation using the following command:

```
mc admin trace --call rebalance ALIAS
```

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

The [`mc admin rebalance`](#command-mc.admin.rebalance) command has the following subcommands:

Subcommand

Description

[`mc admin rebalance start`](#mc.admin.rebalance.start)

Starts a rebalance operation on a MinIO deployment.

[`mc admin rebalance status`](#mc.admin.rebalance.status)

Outputs the current status of an in-progress rebalance operation.

[`mc admin rebalance stop`](#mc.admin.rebalance.stop)

Stops an in-progress rebalance operation.

## Syntax

mc admin rebalance start[](#mc.admin.rebalance.start)

Start a rebalance operation for a MinIO deployment.

EXAMPLES

Consider a MinIO deployment with two pools with an assigned alias of `minio1`.
One pool has 250 GB of free space while the other pool has 3 TB of free space.

The [`mc admin rebalance`](#command-mc.admin.rebalance) command shifts objects from the pool with less free space to the pool with more free space so that there is roughly equal free space on both pools.

```
mc admin rebalance start minio1
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin rebalance start ALIAS
```

Replace ALIAS with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to rebalance.

mc admin rebalance status[](#mc.admin.rebalance.status)

Queries the deployment with an active rebalance process and returns information about the status of the rebalance process.

The status returns the ID of the rebalance operation, the time of the operation, and details for each pool on the deployment.
For each pool, the status shows the pool ID, the pool’s rebalance status, the percentage of used space, and rebalance progress for the pool.

EXAMPLE

```
mc admin rebalance status minio1
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin rebalance ALIAS
```

Replace ALIAS with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

mc admin rebalance stop[](#mc.admin.rebalance.stop)

Ends an in-progress rebalance job on the specified deployment.

EXAMPLES

```
mc admin rebalance stop minio1
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin rebalance stop ALIAS
```

Replace ALIAS with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

## Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Behavior

### Back Up Cluster Settings First

Use the [[reference/minio-mc-admin/mc-admin-cluster-bucket-export#command-mc.admin.cluster.bucket.export|`mc admin cluster bucket export`]] and [[reference/minio-mc-admin/mc-admin-cluster-iam-export#command-mc.admin.cluster.iam.export|`mc admin cluster iam export`]] commands to take a snapshot of the bucket metadata and IAM configurations respectively prior to starting decommissioning.
You can use these snapshots to restore bucket/IAM settings to recover from user or process errors as necessary.

### Rebalancing Ignores Expired Objects and Trailing DeleteMarker

Starting with [RELEASE.2023-06-23T20-26-00Z](https://github.com/minio/minio/releases/tag/RELEASE.2023-06-23T20-26-00Z), rebalancing ignores object versions which have expired based on the configured [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|lifecycle rules]] for the parent bucket.

Rebalancing also ignores objects where the only remaining version is a [[administration/object-management/object-versioning#minio-bucket-versioning-delete|delete marker]].
This avoids inter-pool I/O for objects already considered fully deleted.

MinIO relies on the [[operations/concepts/scanner#minio-concepts-scanner|scanner]] to capture and remove those expired objects or trailing `DeleteMarker` objects.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).