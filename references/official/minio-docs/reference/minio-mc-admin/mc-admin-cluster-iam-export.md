---
title: "mc admin cluster iam export — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-cluster-iam-export.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-cluster-iam-export"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin cluster iam export — SILO (MinIO Object Storage)"
---
# mc admin cluster iam export

## Description

New in version RELEASE.2022-06-26T18-51-48Z.

The [`mc admin cluster iam export`](#command-mc.admin.cluster.iam.export) command exports [[administration/identity-access-management#minio-authentication-and-identity-management|IAM]] metadata for use with the [[reference/minio-mc-admin/mc-admin-cluster-iam-import#command-mc.admin.cluster.iam.import|`mc admin cluster iam import`]] command.

The command saves the output as `ALIAS-iam-metadata.zip`, where `ALIAS` is the [`alias`](#mc.admin.cluster.iam.export.ALIAS) of the MinIO deployment.

EXAMPLE

The following command exports all IAM metadata for the `myminio` deployment.

```
mc admin cluster iam export myminio
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin cluster iam export ALIAS  \
                 [--output, -o <string>]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

Starting with [RELEASE.2023-05-04T18-10-16Z](https://github.com/minio/mc/releases/tag/RELEASE.2023-05-04T18-10-16Z), [`mc admin cluster iam export`](#command-mc.admin.cluster.iam.export) adds support for aliases ending with a trailing forward slash `ALIAS/`.
Prior to this release, the command would fail when provided a trailing forward slash.

### Parameters

ALIAS[](#mc.admin.cluster.iam.export.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment to export IAM metadata for.

--output, --o[](#mc.admin.cluster.iam.export.-output)
Optional

Specify a custom file and path to use when exporting the IAM data.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Download all IAM metadata for a cluster to a ZIP file

The following command downloads all IAM metadata for the cluster at alias `myminio`, then stores the metadata to a ZIP file.

```
mc admin cluster iam export myminio
```

The ZIP file is named `<alias>-iam-info.zip` where `<alias>` is the alias of the cluster.
For the above example, the file is named `myminio-iam-info.zip`.

The file is placed in the current active directory path.

### Download all IAM metadata for a cluster and specify the name and path of the ZIP file

The following command downloads all IAM metadata for the cluster at alias `myminio`, then stores the metadata to a ZIP file at `/tmp/myminio-iam.zip`.

```
mc admin cluster iam export myminio --output /tmp/myminio-iam.zip
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).