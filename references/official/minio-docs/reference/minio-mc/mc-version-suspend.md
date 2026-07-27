---
title: "mc version suspend — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-version-suspend.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-version-suspend"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc version suspend — SILO (MinIO Object Storage)"
---
# mc version suspend

## Syntax

The [`mc version suspend`](#command-mc.version.suspend) command disables versioning on the specified bucket.

EXAMPLE

The following command disables versioning for the `mybucket` bucket on the `myminio` MinIO deployment:

```
mc version suspend myminio/mybucket
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] version suspend ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.version.suspend.ALIAS)

The full path to the bucket on which to disable versioning.
For example:

```
mc version suspend myminio/mybucket
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Example

### Disable Bucket Versioning

Use [`mc version suspend`](#command-mc.version.suspend) to disable versioning for a bucket:

```
mc version suspend ALIAS/PATH
```

Replace [`ALIAS`](#mc.version.suspend.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

Replace [`PATH`](#mc.version.suspend.ALIAS) with the bucket on which to disable versioning.

## Behavior

### Bucket Versioning with Existing Data

Disabling bucket versioning on a bucket with existing versioned data does not remove any versioned objects.
Applications can continue to access versioned data after disabling bucket versioning.
Use [[reference/minio-mc/mc-rm#mc.rm.-versions|`mc rm --versions ALIAS/BUCKET/OBJECT`]] to delete an object and all its versions.

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