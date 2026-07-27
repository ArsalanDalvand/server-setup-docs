---
title: "mc admin cluster bucket export — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-cluster-bucket-export.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-cluster-bucket-export"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin cluster bucket export — SILO (MinIO Object Storage)"
---
# mc admin cluster bucket export

## Description

New in version RELEASE.2022-06-17T02-52-50Z.

The [`mc admin cluster bucket export`](#command-mc.admin.cluster.bucket.export) command exports bucket metadata for use with the [[reference/minio-mc-admin/mc-admin-cluster-bucket-import#command-mc.admin.cluster.bucket.import|`mc admin cluster bucket import`]] command.

You can use this command to manually back up the metadata for the specified MinIO bucket.
The command always saves the output as `cluster-metadata.zip`.

If you specify only the deployment as the target, this command backs up all bucket metadata on the target deployment.

EXAMPLE

The following command exports all bucket metadata for the `myminio` deployment.

```
mc admin cluster bucket export myminio
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin cluster bucket export  \
                                      ALIAS[/BUCKET]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.cluster.bucket.export.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

BUCKET[](#mc.admin.cluster.bucket.export.BUCKET)
Optional

The bucket to export metadata for.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).