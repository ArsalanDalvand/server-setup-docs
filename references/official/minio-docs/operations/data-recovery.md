---
title: "Recover after Hardware Failure — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/data-recovery.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/data-recovery"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Recover after Hardware Failure — SILO (MinIO Object Storage)"
---
# Recover after Hardware Failure

Distributed MinIO deployments rely on [[operations/concepts/erasure-coding#minio-erasure-coding|Erasure Coding]] to provide built-in tolerance for multiple drive or node
failures. Depending on the deployment topology and the selected erasure code
parity, MinIO can tolerate the loss of up to half the drives or nodes in the
deployment while maintaining read access (“read quorum”) to objects.

The following table lists the typical types of failure in a MinIO deployment
and links to procedures for recovering from each:

Failure Type

Description

[[operations/data-recovery/recover-after-drive-failure#minio-restore-hardware-failure-drive|Drive Failure]]

MinIO supports hot-swapping failed drives with new healthy drives.

[[operations/data-recovery/recover-after-node-failure#minio-restore-hardware-failure-node|Node Failure]]

MinIO detects when a node rejoins the deployment and begins proactively [[operations/concepts/healing#minio-concepts-healing|healing]] the node shortly after it is joined back to the cluster healing data previously stored on that node.

[[operations/data-recovery/recover-after-site-failure#minio-restore-hardware-failure-site|Site Failure]]

MinIO Site Replication supports complete resynchronization of buckets, objects, and replication-eligible configuration settings after total site loss.

Since MinIO can operate in a degraded state without significant performance
loss, administrators can schedule hardware replacement in proportion to the rate
of hardware failure. “Normal” failure rates (single drive or node failure) may
allow for a more reasonable replacement timeframe, while “critical” failure
rates (multiple drives or nodes) may require a faster response.

For nodes with one or more drives that are either partially failed or operating
in a degraded state (increasing drive errors, SMART warnings, timeouts in MinIO
logs, etc.), you can safely unmount the drive if the cluster has sufficient
remaining healthy drives to maintain
[[operations/concepts/erasure-coding#minio-ec-parity|read and write quorum]]. Missing drives are less
disruptive to the deployment than drives that are consistently producing read and
write errors.

Exclusive access to drives

MinIO requires exclusive access to the drives or volumes provided for object storage.
No other processes, software, scripts, or persons should perform any actions directly on the drives or volumes provided to MinIO or the objects or files MinIO places on them.

Unless directed by MinIO Engineering, do not use scripts or tools to directly modify, delete, or move any of the data shards, parity shards, or metadata files on the provided drives, including from one drive or node to another.
Such operations are very likely to result in widespread corruption and data loss beyond MinIO’s ability to heal.

MinIO Professional Support

[MinIO SUBNET](https://min.io/pricing?jmp=docs) users can
[log in](https://subnet.min.io/) and create a new issue related to drive, node, or site failures.
Coordination with MinIO Engineering via SUBNET can ensure successful recovery operations of production MinIO deployments, including root-cause analysis, and health diagnostics.

Community users can seek support on the [MinIO Community Slack](https://slack.min.io).
Community Support is best-effort only and has no SLAs around responsiveness.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).