---
title: "Transition Objects to Remote MinIO Deployment — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/object-management/transition-objects-to-minio.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/object-management/transition-objects-to-minio"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Transition Objects to Remote MinIO Deployment — SILO (MinIO Object Storage)"
---
# Transition Objects to Remote MinIO Deployment

The procedure on this page creates a new object lifecycle management rule that transitions objects from a bucket on a primary MinIO deployment to a bucket on a remote MinIO deployment.
This procedure supports cost-management strategies such as tiering objects from a “hot” MinIO deployment using NVMe storage to a “warm” MinIO deployment using SSD.

## Requirements

### Install and Configure mc

This procedure uses [[reference/minio-mc#command-mc|`mc`]] for performing operations on the MinIO cluster.
Install [[reference/minio-mc#command-mc|`mc`]] on a machine with network access to both source and destination
clusters. See the `mc` [[reference/minio-mc#mc-install|Installation Quickstart]] for
instructions on downloading and installing `mc`.

Use the [[reference/minio-mc/mc-alias-set#command-mc.alias.set|`mc alias set`]] command to create an alias for the source MinIO cluster.
Alias creation requires specifying an access key for a user on the source and
destination clusters. The specified users must have [permissions](#minio-lifecycle-management-transition-to-minio-permissions) for configuring and
applying transition operations.

### Required Source MinIO Permissions

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
transition lifecycle management rules on any bucket in the cluster:

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

### Required Remote MinIO Permissions

Object transition lifecycle management rules require additional permissions
on the remote storage tier. Specifically, MinIO requires the remote
tier credentials provide read, write, list, and delete permissions for the
remote bucket.

For example, the following policy on the remote MinIO deployment provides the necessary permission
for transitioning objects into and out of the remote tier:

```
{
   "Version": "2012-10-17",
   "Statement": [
      {
            "Action": [
               "s3:ListBucket"
            ],
            "Effect": "Allow",
            "Resource": [
               "arn:aws:s3:::MyDestinationBucket"
            ],
            "Sid": ""
      },
      {
            "Action": [
               "s3:GetObject",
               "s3:PutObject",
               "s3:DeleteObject"
            ],
            "Effect": "Allow",
            "Resource": [
               "arn:aws:s3:::MyDestinationBucket/*"
            ],
            "Sid": ""
      }
   ]
}
```

Modify the `Resource` for the bucket into which MinIO tiers objects.

Refer to the [[administration/identity-access-management/policy-based-access-control#minio-policy|Access Management]] documentation for more complete guidance on configuring the required permissions.

### Remote Bucket Must Exist

Create the remote bucket prior to configuring lifecycle management tiers or rules using that bucket as the target.

If the remote bucket contains existing data, use the [[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.-prefix|`prefix`]] feature to isolate transitioned objects from any other objects on that bucket.

## Considerations

### Lifecycle Management Object Scanner

MinIO uses a [[operations/concepts/scanner#minio-concepts-scanner|scanner process]] to check objects against all configured
lifecycle management rules. Slow scanning due to high IO workloads or
limited system resources may delay application of lifecycle management rules.

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

## Procedure

### 1) Configure User Accounts and Policies for Lifecycle Management

This step creates users and policies on the MinIO deployment for supporting
lifecycle management operations. You can skip this step if the deployment
already has users with the necessary [permissions](#minio-lifecycle-management-transition-to-minio-permissions).

The following example uses `Alpha` as a placeholder [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] for
the MinIO deployment. Replace this value with the appropriate alias for the
MinIO deployment on which you are configuring lifecycle management rules.
Replace the password `LongRandomSecretKey` with a long, random, and secure
secret key as per your organizations best practices for password generation.

```
wget -O - https://silo.pigsty.io/examples/LifecycleManagementAdmin.json | \
mc admin policy create Alpha LifecycleAdminPolicy /dev/stdin
mc admin user add Alpha alphaLifecycleAdmin LongRandomSecretKey
mc admin policy attach Alpha LifecycleAdminPolicy --user=alphaLifecycleAdmin
```

This example assumes that the specified
aliases have the necessary permissions for creating policies and users
on the deployment. See [[administration/identity-access-management/minio-user-management#minio-users|User Management]] and [[administration/identity-access-management/policy-based-access-control#minio-policy|MinIO Policy Based Access Control]] for more
complete documentation on MinIO users and policies respectively.

### 2) Configure the Remote Storage Tier

Use the [[reference/minio-mc/mc-ilm-tier-add#command-mc.ilm.tier.add|`mc ilm tier add`]] command to add the remote MinIO deployment as the
new remote storage tier:

```
mc ilm tier add minio TARGET TIER_NAME  \
   --endpoint https://HOSTNAME       \
   --access-key ACCESS_KEY           \
   --secret-key SECRET_KEY           \
   --bucket BUCKET                   \
   --prefix PREFIX                   \
   --storage-class STORAGE_CLASS     \
   --region REGION
```

The example above uses the following arguments:

Argument

Description

[[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.TARGET|`ALIAS`]]

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment on which to configure
the MinIO remote tier.

[[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.TIER_NAME|`TIER_NAME`]]

The name to associate with the new MinIO remote storage tier. Specify the
name in all-caps, e.g. `MINIO_WARM_TIER`. This value is required in the next
step.

[[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.-endpoint|`HOSTNAME`]]

The URL endpoint for the MinIO storage backend.

[[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.-access-key|`ACCESS_KEY`]]

The access key MinIO uses to access the bucket. The
access key must correspond to an IAM user with the
required
[permissions](#minio-lifecycle-management-transition-to-minio-permissions-remote).

[[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.-secret-key|`SECRET_KEY`]]

The corresponding secret key for the specified `ACCESS_KEY`.

[[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.-bucket|`BUCKET`]]

The name of the bucket on the remote MinIO deployment to which the `SOURCE`
transitions objects.

[[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.-prefix|`PREFIX`]]

The optional bucket prefix within which MinIO transitions objects.

MinIO stores all transitioned objects in the specified `BUCKET` under a
unique per-deployment prefix value. Omit this argument to use only that
value for isolating and organizing data within the remote storage.

MinIO recommends specifying this optional prefix for remote storage tiers
which contain other data, including transitioned objects from other MinIO
deployments. This prefix should provide a clear reference back to the
source MinIO deployment to facilitate ease of operations related to
diagnostics, maintenance, or disaster recovery.

[[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.-storage-class|`STORAGE_CLASS`]]

The [[reference/minio-server/settings/storage-class#minio-ec-storage-class|Erasure Coding storage class]] MinIO applies to objects transitions to the remote MinIO bucket.
Specify one of the following supported storage classes:

`STANDARD` Recommended

`REDUCED`

[[reference/minio-mc/mc-ilm-tier-add#mc.ilm.tier.add.-region|`REGION`]]

The MinIO region of the specified `BUCKET`.

MinIO deployments typically do not require setting a region as part of setup.
Only include this option if you explicitly set the `MINIO_SITE_REGION` configuration setting for the deployment.

### 3) Create and Apply the Transition Rule

Use the [[reference/minio-mc/mc-ilm-rule-add#command-mc.ilm.rule.add|`mc ilm rule add`]] command to create a new transition rule
for the bucket. The following example configures transition after the
specified number of calendar days:

```
mc ilm rule add ALIAS/BUCKET \
--transition-tier TIERNAME \
--transition-days DAYS \
--noncurrent-transition-days NONCURRENT_DAYS
--noncurrent-transition-tier TIERNAME
```

The example above specifies the following arguments:

Argument

Description

[[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.ALIAS|`ALIAS`]]

Specify the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment for which
you are creating the lifecycle management rule.

[[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.ALIAS|`BUCKET`]]

Specify the full path to the bucket for which you are
creating the lifecycle management rule.

[[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-transition-tier|`TIERNAME`]]

The remote storage tier to which MinIO transitions objects.
Specify the remote storage tier name created in the previous step.

If you want to transition noncurrent object versions to a distinct
remote tier, specify a different tier name for
[[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-noncurrent-transition-tier|`--noncurrent-transition-tier`]].

[[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-transition-days|`DAYS`]]

The number of calendar days after which MinIO marks an object as
eligible for transition. Specify the number of days as an integer,
e.g. `30` for 30 days.

[[reference/minio-mc/mc-ilm-rule-add#mc.ilm.rule.add.-noncurrent-transition-days|`NONCURRENT_DAYS`]]

The number of calendar days after which MinIO marks a noncurrent
object version as eligible for transition. MinIO specifically measures
the time since an object became non-current instead of the object
creation time. Specify the number of days as an integer,
e.g. `90` for 90 days.

Omit this value to ignore noncurrent object versions.

This option has no effect on non-versioned buckets.

### 4) Verify the Transition Rule

Use the [[reference/minio-mc/mc-ilm-rule-ls#command-mc.ilm.rule.ls|`mc ilm rule ls`]] command to review the configured transition
rules:

```
mc ilm rule ls ALIAS/PATH --transition
```

Replace [[reference/minio-mc/mc-ilm-rule-ls#mc.ilm.rule.ls.ALIAS|`ALIAS`]] with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]]
of the MinIO deployment.

Replace [[reference/minio-mc/mc-ilm-rule-ls#mc.ilm.rule.ls.ALIAS|`PATH`]] with the name of the bucket for
which to retrieve the configured lifecycle management rules.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).