---
title: "mc admin accesskey disable — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-accesskey-disable.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-accesskey-disable"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin accesskey disable — SILO (MinIO Object Storage)"
---
# mc admin accesskey disable

## Syntax

The [`mc admin accesskey disable`](#command-mc.admin.accesskey.disable) command disables an existing access key for a MinIO IDP user.

EXAMPLE

The following command disables the specified access key:

```
mc admin accesskey disable myminio myuserserviceaccount
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin accesskey disable         \
                                 ALIAS           \
                                 SERVICEACCOUNT
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.accesskey.disable.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

SERVICEACCOUNT[](#mc.admin.accesskey.disable.SERVICEACCOUNT)
Required

The access key to disable.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Behavior

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).