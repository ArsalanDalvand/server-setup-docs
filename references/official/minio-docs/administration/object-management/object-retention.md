---
title: "MinIO Object Locking — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/object-management/object-retention.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/object-management/object-retention"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO Object Locking — SILO (MinIO Object Storage)"
---
# MinIO Object Locking

[Object locking and retention overview](https://youtu.be/Hk9Z-sltUu8?ref=docs)

[Object locking and retention lab](https://youtu.be/thNus-DL1u4?ref=docs)

## Overview

MinIO Object Locking (“Object Retention”) enforces Write-Once Read-Many (WORM)
immutability to protect [[administration/object-management/object-versioning#minio-bucket-versioning|versioned objects]] from
deletion. MinIO supports both
[duration based object retention](#minio-object-locking-retention-modes)
and
[indefinite legal hold retention](#minio-object-locking-legalhold).

MinIO Object Locking provides key data retention compliance and meets
SEC17a-4(f), FINRA 4511(C), and CFTC 1.31(c)-(d) requirements as per
[Cohasset Associates](https://min.io/cohasset?ref-docs).

Bucket Without Locking
[](../../_images/minio-versioning-delete-object1.svg)

MinIO versioning preserves the full history of object mutations.
However, applications can explicitly delete specific object versions.

Bucket With Locking
[](../../_images/minio-object-locking1.svg)

Applying a default 30 Day WORM lock to objects in the bucket ensures
a minimum period of retention and protection for all object versions.

Delete Operations in Locked Bucket
[](../../_images/minio-object-locking-delete1.svg)

[[administration/object-management/object-delete#minio-object-delete|Delete operations]] follow normal behavior in
[[administration/object-management/object-versioning#minio-bucket-versioning-delete|versioned buckets]], where MinIO
creates a `DeleteMarker` for the object. However, non-Delete Marker
versions of the object remain under the retention rules and are protected
from any specific deletion or overwrite attempts.

Versioned Delete Operations in Locked Bucket
[](../../_images/minio-object-locking-delete-version1.svg)

MinIO blocks any attempt to [[administration/object-management/object-delete#minio-object-delete|delete]] a specific object version held under
WORM lock. The earliest possible time after which a client may delete
the version is when the lock expires.

MinIO object locking is
[feature and API compatible with AWS S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html).
This page summarizes Object Locking / Retention concepts as implemented by
MinIO. See the AWS S3 documentation on
[How S3 Object Lock works](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html) for additional
resources.

You can only enable object locking during bucket creation as per
[S3 behavior](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock-overview.html#object-lock-bucket-config).
You cannot enable object locking on a bucket created without locking
enabled. You can then configure object retention rules at any time.
Object locking requires [[administration/object-management/object-versioning#minio-bucket-versioning|versioning]] and
enables the feature implicitly.

### Interaction with Versioning

Objects held under WORM locked are immutable until the lock expires or is
explicitly lifted. Locking is per-object version, where each version is
independently immutable.

If an application performs an unversioned delete operation on a locked object,
the operation produces a [[administration/object-management/object-versioning#minio-bucket-versioning-delete|delete marker]].
Attempts to explicitly delete any WORM-locked object fail with an error.
Delete Markers are not eligible for protection under WORM locking.
See the S3 documentation on
[Managing delete markers and object lifecycles](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock-managing.html#object-lock-managing-lifecycle) for more
information.

For example, consider the following bucket with
[GOVERNANCE Mode](#minio-object-locking-governance) locking enabled by default:

```
$ mc ls --versions play/locking-guide

  [DATETIME]    29B 62429eb1-9cb7-4dc5-b507-9cc23d0cc691 v3 PUT data.csv
  [DATETIME]    32B 78b3105a-02a1-4763-8054-e66add087710 v2 PUT data.csv
  [DATETIME]    23B c6b581ca-2883-41e2-9905-0a1867b535b8 v1 PUT data.csv
```

Attempting to perform a delete on a specific version of `data.csv` fails
due to the object locking settings:

```
$ mc rm --version-id 62429eb1-9cb7-4dc5-b507-9cc23d0cc691 play/data.csv

  Removing `play/locking-guide/data.csv` (versionId=62429eb1-9cb7-4dc5-b507-9cc23d0cc691).
  mc: <ERROR> Failed to remove `play/locking-guide/data.csv`.
      Object, 'data.csv (Version ID=62429eb1-9cb7-4dc5-b507-9cc23d0cc691)' is
      WORM protected and cannot be overwritten
```

Attempting to perform an unversioned delete on `data.csv` succeeds and creates
a new `DeleteMarker` for the object:

```
$ mc rm play/locking-guide/data.csv

  [DATETIME]     0B acce329f-ad32-46d9-8649-5fe8bf4ec6e0 v4 DEL data.csv
  [DATETIME]    29B 62429eb1-9cb7-4dc5-b507-9cc23d0cc691 v3 PUT data.csv
  [DATETIME]    32B 78b3105a-02a1-4763-8054-e66add087710 v2 PUT data.csv
  [DATETIME]    23B c6b581ca-2883-41e2-9905-0a1867b535b8 v1 PUT data.csv
```

### Interaction with Lifecycle Management

MinIO [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|object expiration]]
respects any active object lock and retention settings for objects covered by
the expiration rule.

For expiration rules operating on only the current object version,
MinIO creates a Delete Marker for the locked object.

For expiration rules operating on non-current object versions,
MinIO can only expire the non-current versions after the retention period
has passed or has been explicitly lifted (e.g. legal holds).

For example, consider the following bucket with
[GOVERNANCE Mode](#minio-object-locking-governance) locking enabled by default for 45 days:

```
$ mc ls --versions play/locking-guide

  [7D]    29B 62429eb1-9cb7-4dc5-b507-9cc23d0cc691 v3 PUT data.csv
  [30D]    32B 78b3105a-02a1-4763-8054-e66add087710 v2 PUT data.csv
  [60D]    23B c6b581ca-2883-41e2-9905-0a1867b535b8 v1 PUT data.csv
```

Creating an expiration rule for current objects older than 7 days results in
a Delete Marker for the object:

```
$ mc ls --versions play/locking-guide

  [0D]     0B acce329f-ad32-46d9-8649-5fe8bf4ec6e0 v4 DEL data.csv
  [7D]    29B 62429eb1-9cb7-4dc5-b507-9cc23d0cc691 v3 PUT data.csv
  [30D]    32B 78b3105a-02a1-4763-8054-e66add087710 v2 PUT data.csv
  [60D]    23B c6b581ca-2883-41e2-9905-0a1867b535b8 v1 PUT data.csv
```

However, an expiration rule for non-current objects older than 7 days would
only take effect after the configured WORM lock expires. Since the bucket
has a 45 day `GOVERNANCE` retention set, only the `v1` version of
`data.csv` is unlocked and therefore eligible for deletion.

## Tutorials

### Create Bucket with Object Locking Enabled

You must enable object locking during bucket creation as per S3 behavior.
You can create a bucket with object locking enabled using the MinIO [[reference/minio-mc#command-mc|`mc`]] CLI or using an S3-compatible SDK.

Use the [[reference/minio-mc/mc-mb#command-mc.mb|`mc mb`]] command with the [[reference/minio-mc/mc-mb#mc.mb.-with-lock|`--with-lock`]]
option to create a bucket with object locking enabled:

```
mc mb --with-lock ALIAS/BUCKET
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured
MinIO deployment.

Replace `BUCKET` with the
[[reference/minio-mc/mc-mb#mc.mb.ALIAS|`name`]] of the bucket to create.

### Configure Bucket-Default Object Retention

You can configure object locking rules (“object retention”) using the MinIO [[reference/minio-mc#command-mc|`mc`]] CLI, or using an S3-compatible SDK.

MinIO supports setting both bucket-default and per-object retention rules.
The following examples set bucket-default retention. For per-object retention
settings, defer to the documentation for the `PUT` operation used by your
preferred SDK.

Use the [[reference/minio-mc/mc-retention-set#command-mc.retention.set|`mc retention set`]] command with the
[[reference/minio-mc/mc-retention-set#mc.retention.set.-recursive|`--recursive`]] and
[[reference/minio-mc/mc-retention-set#mc.retention.set.-default|`--default`]] options to set the
default retention mode for a bucket:

```
mc retention set --recursive --default MODE DURATION ALIAS/BUCKET
```

Replace [[reference/minio-mc/mc-retention-set#mc.retention.set.MODE|`MODE`]] with either either [COMPLIANCE](#minio-object-locking-compliance) or [GOVERNANCE](#minio-object-locking-governance).

Replace [[reference/minio-mc/mc-retention-set#mc.retention.set.VALIDITY|`DURATION`]] with the duration for which the object lock remains in effect.

Replace [[reference/minio-mc/mc-retention-set#mc.retention.set.ALIAS|`ALIAS`]] with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

Replace [[reference/minio-mc/mc-retention-set#mc.retention.set.ALIAS|`BUCKET`]] with the name of the bucket on which to set the default retention rule.

### Enable Legal Hold Retention

You can enable or disable indefinite legal hold retention for an object using the MinIO [[reference/minio-mc#command-mc|`mc`]] CLI or using an S3-compatible SDK.

You can place a legal hold on an object already held under a [COMPLIANCE](#minio-object-locking-compliance) or [GOVERNANCE](#minio-object-locking-governance) lock.
The object remains WORM locked under the legal hold even when the retention lock expires.
You or another user with the necessary permissions must explicitly lift the legal hold to remove the WORM lock.

Use the mc legalhold set command to toggle the legal hold status on an object.

```
mc legalhold set ALIAS/PATH
```

Replace [[reference/minio-mc/mc-legalhold-set#mc.legalhold.set.ALIAS|`ALIAS`]] with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

Replace [[reference/minio-mc/mc-legalhold-set#mc.legalhold.set.ALIAS|`PATH`]] with the path to the object for which to enable the legal hold.

## Object Retention Modes

MinIO implements the following
[S3 Object Locking Modes](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock-overview.html):

Mode

Summary

[GOVERNANCE Mode](#minio-object-locking-governance)

Prevents any operation that would mutate or modify the object or its
locking settings by non-privileged users.

Users with the [[administration/identity-access-management/policy-based-access-control#policy-action.s3-BypassGovernanceRetention|`s3:BypassGovernanceRetention`]] permission
on the bucket or object can modify the object or its locking settings.

MinIO lifts the lock automatically after the configured retention rule
duration has passed.

[COMPLIANCE Mode](#minio-object-locking-compliance)

Prevents any operation that would mutate or modify the object or its
locking settings.

No MinIO user can modify the object or its settings, including the
[[administration/identity-access-management/minio-user-management#minio-users-root|MinIO root]] user.

MinIO lifts the lock automatically after the configured retention rule
duration has passed.

### GOVERNANCE Mode

An object under `GOVERNANCE` lock is protected from write operations by
non-privileged users.

`GOVERNANCE` locked objects enforce managed-immutability for locked objects,
where users with the [[administration/identity-access-management/policy-based-access-control#policy-action.s3-BypassGovernanceRetention|`s3:BypassGovernanceRetention`]] action can
modify the locked object, change the retention duration, or lift the lock
entirely. Bypassing `GOVERNANCE` retention also requires setting the
`x-amz-bypass-governance-retention:true` header as part of the request.

The MinIO `GOVERNANCE` lock is functionally identical to the
[S3 GOVERNANCE mode](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html#object-lock-retention-modes).

### COMPLIANCE Mode

An object under `COMPLIANCE` lock is protected from write operations by all
users, including the [[administration/identity-access-management/minio-user-management#minio-users-root|MinIO root]] user.

`COMPLIANCE` locked objects enforce complete immutability for locked objects.
You cannot change or remove the lock before the configured retention
duration has passed.

The MinIO `COMPLIANCE` lock is functionally identical to the
[S3 COMPLIANCE mode](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html#object-lock-retention-modes).

## Legal Hold

An object under legal hold is protected from write operations by all
users, including the [[administration/identity-access-management/minio-user-management#minio-users-root|MinIO root]] user.

Legal holds are indefinite and enforce complete immutability for locked objects.
Only privileged users with the [[administration/identity-access-management/policy-based-access-control#policy-action.s3-PutObjectLegalHold|`s3:PutObjectLegalHold`]] permission can set or lift the legal hold.

Legal holds apply at the object level.
If you enable legal hold for a group of objects, such as the contents of a bucket, subsequently created objects in that bucket are not affected.

Legal holds are complementary to both [GOVERNANCE Mode](#minio-object-locking-governance) and [COMPLIANCE Mode](#minio-object-locking-compliance) retention settings.
An object held under both legal hold and a `GOVERNANCE/COMPLIANCE` retention rule remains WORM locked until the legal hold is lifted and the rule expires.

For `GOVERNANCE` locked objects, the legal hold prevents mutating the object even if the user has the necessary privileges to bypass retention.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).