---
title: "Object Lifecycle Management — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/object-management/object-lifecycle-management.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/object-management/object-lifecycle-management"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Object Lifecycle Management — SILO (MinIO Object Storage)"
---
# Object Lifecycle Management

[MinIO Object Lifecycle Management Part I](https://youtu.be/Exg2KsfzHzI?ref=docs)

[MinIO Object Lifecycle Management Part II](https://youtu.be/5fz3rE3wjGg?ref=docs)

[MinIO Object Lifecycle Management Lab](https://youtu.be/5fz3rE3wjGg?ref=docs)

Use MinIO Object Lifecycle Management to create rules for time or date based automatic transition or expiry of objects.
For object transition, MinIO automatically moves the object to a configured remote storage tier.
For object expiry, MinIO automatically deletes the object.

MinIO derives it’s behavior and syntax from [S3 lifecycle](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lifecycle-mgmt.html) for compatibility in migrating workloads and lifecycle rules from S3 to MinIO.
For example, you can export S3 lifecycle management rules and import them into MinIO or vice-versa.
MinIO uses JSON to describe lifecycle management rules and may require conversion to or from XML as part of importing S3 lifecycle rules.

## Object Transition (“Tiering”)

MinIO supports creating object transition lifecycle management rules, where MinIO can automatically move an object to a remote storage “tier”.
MinIO supports any of the following remote tier targets:

[[administration/object-management/transition-objects-to-minio#minio-lifecycle-management-transition-to-minio|MinIO]]

[[administration/object-management/transition-objects-to-s3#minio-lifecycle-management-transition-to-s3|Amazon S3]]

[[administration/object-management/transition-objects-to-gcs#minio-lifecycle-management-transition-to-gcs|Google Cloud Storage]]

[[administration/object-management/transition-objects-to-azure#minio-lifecycle-management-transition-to-azure|Microsoft Azure Blob Storage]]

MinIO object transition supports use cases like moving aged data from MinIO clusters in private or public cloud infrastructure to low-cost private or public cloud storage solutions.
Directory objects, which are 0-byte objects with a name ending in `/`, do not tier.
MinIO manages retrieving tiered objects on-the-fly without any additional application-side logic.

Use the [[reference/minio-mc/mc-ilm-tier-add#command-mc.ilm.tier.add|`mc ilm tier add`]] command to create a remote target for tiering data to that target.
You can then use the [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-transition-days|`mc ilm rule add --transition-days`]] command to transition objects to that tier after a specified number of calendar days.

New in version RELEASE.2022-11-10T18-20-21Z.

You can verify the tiering status of an object using [[reference/minio-mc/mc-ls#command-mc.ls|`mc ls`]] against the bucket or bucket prefix.
The output includes the storage tier of each object:

```
$ mc ls play/mybucket
[2022-11-08 11:30:24 PST]    52MB  STANDARD log-data.csv
[2022-11-09 12:20:18 PST]    120MB WARM event-2022-11-09.mp4
```

`STANDARD` marks objects stored on the MinIO deployment.

`WARM` marks objects stored on the remote tier with matching name.

Important

MinIO Object Transition supports cost-saving strategies around moving older or aged data to cost-optimized remote storage tiers, such as cloud storage or high-density HDD storage.

MinIO Object Transition does not provide backup and recovery functionality.
You cannot use the remote tier as a recovery source in the event of data loss in MinIO.

Use either [[operations/replication/multi-site-replication#minio-site-replication-overview|site replication]] or [[administration/bucket-replication#minio-bucket-replication|bucket replication]] to support backup/recovery or BC/DR requirements.

### Exclusive Access to Remote Data

MinIO requires exclusive access to the transitioned data on the remote storage tier.
Object metadata on the “hot” MinIO source is strongly linked to the object data on the “warm/cold” remote tier.
MinIO cannot retrieve object data without access to the remote, nor can the remote be used to restore lost metadata on the source.

All access to the transitioned objects must occur through MinIO via S3 API operations only.
Manually modifying a transitioned object - whether the metadata on the “hot” MinIO tier or the object data on the remote “warm/cold” tier - may result in loss of that object data.

MinIO ignores any objects in the remote bucket or bucket prefix not explicitly managed by the MinIO deployment.
Automatic transition and transparent object retrieval depend on the following assumptions:

No external mutation, migration, or deletion of objects on the remote storage.

No lifecycle management rules (e.g. transition or expiration) on the remote
storage bucket.

MinIO stores all transitioned objects in the remote storage bucket or resource under a unique per-deployment prefix value.
This value is not intended to support identifying the source deployment from the backend.
MinIO supports an additional optional human-readable prefix when configuring the remote target, which may facilitate operations related to diagnostics, maintenance, or disaster recovery.

MinIO recommends specifying this optional prefix for remote storage tiers which contain other data, including transitioned objects from other MinIO deployments.
This tutorial includes the necessary syntax for setting this prefix.

### Availability of Remote Data

MinIO tiering behavior depends on the remote storage returning objects immediately (milliseconds to seconds) upon request.
MinIO therefore cannot support remote storage which requires rehydration, wait periods, or manual intervention.

MinIO creates metadata for each transitioned object that identifies its location on the remote storage.
Applications cannot trivially identify and access a transitioned object independent of MinIO.
Availability of the transitioned data therefore depends on the same core
protections that [[operations/concepts/erasure-coding#minio-erasure-coding|erasure coding]] and distributed
deployment topologies provide for all objects on the MinIO deployment. Using
object transition does not provide any additional business continuity or
disaster recovery benefits.

Workloads that require BC/DR
protections should implement MinIO [[administration/bucket-replication#minio-bucket-replication-serverside|Server-Side replication]]. Replication ensures objects remains
preserved on the remote replication site, such that you can resynchronize from
the remote in the event of partial or total data loss. See
[[administration/bucket-replication#minio-replication-behavior-resync|Resynchronization (Disaster Recovery)]] for more complete documentation on
using replication to recover after partial or total data loss.

### Versioned Buckets

MinIO adopts [S3 behavior](https://docs.aws.amazon.com/AmazonS3/latest/userguide/intro-lifecycle-rules.html#intro-lifecycle-rules-actions) for transition rules on [[administration/object-management/object-versioning#minio-bucket-versioning|versioned buckets]].
Specifically, MinIO by default applies the transition operation to the current object version.

To transition noncurrent object versions, specify the [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-noncurrent-transition-days|`--noncurrent-transition-days`]] and [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-noncurrent-transition-tier|`--noncurrent-transition-tier`]] options when creating the transition rule.

## Object Expiration

MinIO lifecycle management supports expiring objects on a bucket.
Object “expiration” involves performing a `DELETE` operation on the object.
For example, you can create a lifecycle management rule to expire any object older than 365 days.

Use [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-expire-days|`mc ilm rule add --expire-days`]] to expire objects after a specified number of calendar days.

For buckets with [[administration/bucket-replication#minio-bucket-replication|replication]] configured, MinIO does not replicate objects deleted by a lifecycle management expiration rule.
See [[administration/bucket-replication#minio-replication-behavior-delete|Replication of Delete Operations]] for more information.

### Versioned Buckets

MinIO adopts [S3 behavior](https://docs.aws.amazon.com/AmazonS3/latest/userguide/intro-lifecycle-rules.html#intro-lifecycle-rules-actions) for expiration rules on [[administration/object-management/object-versioning#minio-bucket-versioning|versioned buckets]].
MinIO has several default behaviors for versioned buckets:

MinIO applies the expiration option to only the current object version by creating a `DeleteMarker` as is normal with versioned delete.

To expire noncurrent object versions, specify the [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-noncurrent-expire-days|`--noncurrent-expire-days`]] option when creating the expiration rule.

MinIO does not expire `DeleteMarkers` even if no other versions of that object exist.

To expire delete markers when there are no remaining versions for that object, specify the [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-expire-delete-marker|`--expire-delete-marker`]] option when creating the expiration rule.

To expire all versions of an object that does not have a delete marker after a specified period of days, use the [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-expire-all-object-versions|`--expire-all-object-versions`]] flag with the [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-expire-days|`--expire-days`]] flag.
This permits the permanent deletion of the object after the specified number of days pass.

Changed in version MinIO: RELEASE.2024-05-01T01-11-10Z

This flag applies only to objects that do not have a delete marker.

## Lifecycle Management Object Scanner

MinIO uses a built-in [[operations/concepts/scanner#minio-concepts-scanner|scanner]] to actively check objects against all configured lifecycle management rules.

The scanner is a low-priority process that yields to high I/O workloads to prevent performance spikes triggered by rule timing.
The scanner may therefore not detect an object as eligible for a configured transition or expiration lifecycle rule until after the lifecycle rule period has passed.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).