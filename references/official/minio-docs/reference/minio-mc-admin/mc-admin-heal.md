---
title: "mc admin heal — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-heal.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-heal"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin heal — SILO (MinIO Object Storage)"
---
# mc admin heal

## Description

The [`mc admin heal`](#command-mc.admin.heal) command scans for objects that are damaged or corrupted and heals those objects.

[`mc admin heal`](#command-mc.admin.heal) is resource intensive and typically not required as a manual process, even after drive failures or corruption events.

As a part of normal operations, MinIO:

automatically heals objects damaged by silent bit rot corruption, drive failure, or other issues on each `POST` or `GET` operation.

performs periodic background object healing using the [[operations/concepts/scanner#minio-concepts-scanner|scanner]].

aggressively heals objects after drive replacement.

Refer to [[operations/concepts/healing#minio-concepts-healing|Object Healing]] for more details on how MinIO heals objects.

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

## Syntax

[`mc admin heal`](#command-mc.admin.heal) has the following syntax:

```
mc admin heal [FLAGS] TARGET             \
                      [--all-drives, -a] \
                      [--force]          \
                      [--verbose, -v]
```

[`mc admin heal`](#command-mc.admin.heal) supports the following arguments:

TARGET[](#mc.admin.heal.TARGET)
Required

The full path to the bucket or bucket prefix on which the command should perform object healing.
Specify the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment as the prefix for the path.
For example:

```
mc admin heal play/mybucket/myprefix
```

If the `TARGET` bucket or bucket prefix has an active healing scan, the command returns the status of that scan.

--all-drives, -a[](#mc.admin.heal.-all-drives)
Optional

Select all drives and show verbose information.

--force[](#mc.admin.heal.-force)
Optional

Disables warning prompts.

--verbose, -v[](#mc.admin.heal.-verbose)
Optional

Show information about offline and faulty healing drives.

## Healing Colors

Some versions of MinIO used a color key as a way to differentiate objects with different healing statuses.

Changed in version mc: RELEASE.2024-11-17T19-35-25Z

The color meaning has been updated.

Green indicates the bucket is healthy.

Yellow indicates the bucket requires healing on one or more drives.

Red indicates one or more drives are unhealthy.

Grey indicates an indeterminate healing state.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).