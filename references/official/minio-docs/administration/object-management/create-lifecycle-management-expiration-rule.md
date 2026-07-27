---
title: "Automatic Object Expiration — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/object-management/create-lifecycle-management-expiration-rule.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/object-management/create-lifecycle-management-expiration-rule"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Automatic Object Expiration — SILO (MinIO Object Storage)"
---
# Automatic Object Expiration

Each procedure on this page creates a new object lifecycle management rule that
expires objects on a MinIO bucket. This procedure supports use cases like
removing “old” objects after a certain time period or calendar date.

## Requirements

### Install and Configure mc

This procedure uses [[reference/minio-mc#command-mc|`mc`]] for performing operations on the MinIO cluster.
Install [[reference/minio-mc#command-mc|`mc`]] on a machine with network access to both source and destination
clusters. See the `mc` [[reference/minio-mc#mc-install|Installation Quickstart]] for
instructions on downloading and installing `mc`.

Use the [[reference/minio-mc/mc-alias-set#command-mc.alias.set|`mc alias set`]] command to create an alias for the source MinIO cluster
and the destination S3-compatible service. Alias creation requires specifying an
access key for a user on the source and destination clusters. The specified
users must have [permissions](#minio-lifecycle-management-create-expiry-rule-permissions) for configuring
and applying expiry operations.

### Required Permissions

MinIO requires the following permissions scoped to the bucket or buckets
for which you are creating lifecycle management rules.

[[administration/identity-access-management/policy-based-access-control#policy-action.s3-PutLifecycleConfiguration|`s3:PutLifecycleConfiguration`]]

[[administration/identity-access-management/policy-based-access-control#policy-action.s3-GetLifecycleConfiguration|`s3:GetLifecycleConfiguration`]]

MinIO also requires the following administrative permissions on the cluster
in which you are creating remote tiers for object transition lifecycle
management rules:

[[administration/identity-access-management/policy-based-access-control#policy-action.admin-SetTier|`admin:SetTier`]]

[[administration/identity-access-management/policy-based-access-control#policy-action.admin-ListTier|`admin:ListTier`]]

For example, the following policy provides permission for configuring object
transition lifecycle management rules on any bucket in the cluster:.

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
            "Action": [
               "admin:SetTier",
               "admin:ListTier"
            ],
            "Effect": "Allow",
            "Sid": "EnableRemoteTierManagement"
      },
      {
            "Action": [
               "s3:PutLifecycleConfiguration",
               "s3:GetLifecycleConfiguration"
            ],
            "Resource": [
                        "arn:aws:s3:::*"
            ],
            "Effect": "Allow",
            "Sid": "EnableLifecycleManagementRules"
      }
   ]
}
```

## Expire Objects after Number of Days

Use [[reference/minio-mc/mc-ilm-rule-add#command-mc.ilm.rule.add|`mc ilm rule add`]] with [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-expire-days|`--expire-days`]] to
expire bucket contents a number of days after object creation:

```
mc ilm rule add ALIAS/PATH --expire-days "DAYS"
```

Replace [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.ALIAS|`ALIAS`]] with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.ALIAS|`PATH`]] with the path to the bucket on the
S3-compatible host.

Replace [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-expire-days|`DAYS`]] with the number of days after
which to expire the object. For example, specify `30` to expire the
object 30 days after creation.

## Expire Versioned Objects

Use [[reference/minio-mc/mc-ilm-rule-add#command-mc.ilm.rule.add|`mc ilm rule add`]] to expiring noncurrent object versions and object
delete markers:

To expire noncurrent object versions after a specific duration in days,
include [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-noncurrent-expire-days|`--noncurrent-expire-days`]].

To expire delete markers for objects with no remaining versions,
include [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-expire-delete-marker|`--expire-delete-marker`]].

```
mc ilm rule add ALIAS/PATH \
   --noncurrent-expire-days NONCURRENT_DAYS \
   --expire-delete-marker
```

To expire all versions of an object, include [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-expire-all-object-versions|`--expire-all-object-versions`]].
This expiration only applies to objects without a `DeleteMarker` as the latest or current version.

```
mc ilm rule add ALIAS/PATH \
   --expire-all-object-versions
```

Replace [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.ALIAS|`ALIAS`]] with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.ALIAS|`PATH`]] with the path to the bucket on the
S3-compatible host.

Replace [[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-noncurrent-expire-days|`NONCURRENT_DAYS`]] with the number of days after
which to expire noncurrent object versions. For example, specify `30d` to
expire a version after it has been noncurrent for at least 30 days.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).