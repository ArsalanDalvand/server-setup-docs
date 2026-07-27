---
title: "mc version info — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-version-info.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-version-info"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc version info — SILO (MinIO Object Storage)"
---
# mc version info

## Syntax

The [`mc version info`](#command-mc.version.info) command returns the versioning status for the specified bucket.

EXAMPLE

The following command returns the versioning status for the `mybucket` bucket on the `myminio` MinIO deployment:

```
mc version info myminio/mybucket
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] version info ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.version.info.ALIAS)

The full path to the bucket on which to retrieve the versioning status.
For example:

```
mc version info myminio/mybucket
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Example

### Get Bucket Versioning Status

Use [`mc version info`](#command-mc.version.info) to retrieve the versioning status for a bucket:

```
mc version info ALIAS/PATH
```

Replace [`ALIAS`](#mc.version.info.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

Replace [`PATH`](#mc.version.info.ALIAS) with the bucket on which to retrieve the versioning status.

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