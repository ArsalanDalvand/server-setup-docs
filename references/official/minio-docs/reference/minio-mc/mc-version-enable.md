---
title: "mc version enable — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-version-enable.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-version-enable"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc version enable — SILO (MinIO Object Storage)"
---
# mc version enable

## Syntax

The [`mc version enable`](#command-mc.version.enable) command enables versioning on the specified bucket.

EXAMPLE

The following command enables versioning for the `mybucket` bucket on the `myminio` MinIO deployment:

```
mc version enable myminio/mybucket
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] version enable ALIAS                \
                                --exclude-folders    \
                                --excluded-prefixes
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.version.enable.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment and the full path to the bucket for which to enable versioning. For example:

```
mc version enable myminio/mybucket
```

--exclude-folders[](#mc.version.enable.-exclude-folders)
Optional

Disable versioning on all folders (objects whose name ends with `/`) in the specified bucket.

--excluded-prefixes[](#mc.version.enable.-excluded-prefixes)
Optional

Disable versioning on objects matching a list of prefixes, up to 10.
The list of prefixes match all objects containing the specified strings in their prefix or name, similar to a regular expression of the form `prefix*`.
To match objects by prefix only, use `prefix/*`.

For example, the following command excludes any objects containing `_test` or `_temp` in their prefix or name from versioning:

```
mc version enable --excluded-prefixes "_test, _temp" myminio/mybucket
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Example

### Enable Bucket Versioning

Use [`mc version enable`](#command-mc.version.enable) to enable versioning for a bucket:

```
mc version enable ALIAS/PATH
```

Replace [`ALIAS`](#mc.version.enable.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

Replace [`PATH`](#mc.version.enable.ALIAS) with the bucket on which to enable versioning.

## Behavior

### Bucket Versioning with Existing Data

Enabling bucket versioning on a bucket with existing data immediately creates a `NULL` value version ID for each unversioned object.

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