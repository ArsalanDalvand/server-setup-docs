---
title: "mc admin cluster iam import — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-cluster-iam-import.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-cluster-iam-import"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin cluster iam import — SILO (MinIO Object Storage)"
---
# mc admin cluster iam import

## Description

New in version RELEASE.2022-06-17T02-52-50Z.

The [`mc admin cluster iam import`](#command-mc.admin.cluster.iam.import) command imports [[administration/identity-access-management#minio-authentication-and-identity-management|IAM]] metadata as created by the [[reference/minio-mc-admin/mc-admin-cluster-iam-export#command-mc.admin.cluster.iam.export|`mc admin cluster iam export`]] command.

You can use this command to manually restore IAM metadata settings for a MinIO deployment.

New in version mc: RELEASE.2024-09-09T07-53-10Z

The command outputs the results of the import, including the following:

count of individual entities imported by entity type

list of policies imported by entity type they imported to

list of entities that failed to import

EXAMPLE

The following command imports the IAM metadata of the specified file onto the `myminio` deployment.

```
mc admin cluster iam import myminio ~/minio-metadata-backup/myminio-cluster.zip
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin cluster iam import  \
                                   ALIAS \
                                   IAM-METADATA.ZIP
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

Starting with [RELEASE.2023-05-04T18-10-16Z](https://github.com/minio/mc/releases/tag/RELEASE.2023-05-04T18-10-16Z), [`mc admin cluster iam import`](#command-mc.admin.cluster.iam.import) adds support for aliases ending with a trailing forward slash `ALIAS/`.
Prior to this release, the command would fail when provided a trailing forward slash.

### Parameters

ALIAS[](#mc.admin.cluster.iam.import.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

IAM-METADATA.ZIP[](#mc.admin.cluster.iam.import.IAM-METADATA.ZIP)
Required

The path to the IAM metadata file to import.

Use the [[reference/minio-mc-admin/mc-admin-cluster-iam-export#command-mc.admin.cluster.iam.export|`mc admin cluster iam export`]] to export IAM metadata for use with this command.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).