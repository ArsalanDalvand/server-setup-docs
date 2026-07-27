---
title: "mc admin cluster bucket import — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-cluster-bucket-import.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-cluster-bucket-import"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin cluster bucket import — SILO (MinIO Object Storage)"
---
# mc admin cluster bucket import

## Description

New in version RELEASE.2022-06-17T02-52-50Z.

The [`mc admin cluster bucket import`](#command-mc.admin.cluster.bucket.import) command imports bucket metadata as created by the [[reference/minio-mc-admin/mc-admin-cluster-bucket-export#command-mc.admin.cluster.bucket.export|`mc admin cluster bucket export`]] command.

You can use this command to manually restore the metadata to the specified bucket on a MinIO deployment.

If you specify only the deployment as the target, this command applies the metadata objects to all matching buckets on the target.

EXAMPLE

The following command imports the specified metadata to the `myminio` deployment.

```
mc admin cluster bucket import myminio ~/minio-metadata-backup/myminio-cluster.zip
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin cluster bucket import  \
                                    ALIAS[/BUCKET] \
                                    METADATA.ZIP
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.cluster.bucket.import.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

METADATA.ZIP[](#mc.admin.cluster.bucket.import.METADATA.ZIP)
Required

The path to the metadata file to import.

Use [[reference/minio-mc-admin/mc-admin-cluster-bucket-export#command-mc.admin.cluster.bucket.export|`mc admin cluster bucket export`]] to export bucket metadata for use with this command.

BUCKET[](#mc.admin.cluster.bucket.import.BUCKET)
Optional

The bucket to apply the imported metadata to.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).