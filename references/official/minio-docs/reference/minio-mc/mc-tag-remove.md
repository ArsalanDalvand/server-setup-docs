---
title: "mc tag remove — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-tag-remove.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-tag-remove"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc tag remove — SILO (MinIO Object Storage)"
---
# mc tag remove

## Syntax

The [`mc tag remove`](#command-mc.tag.remove) command removes all tags from a bucket or object.

EXAMPLE

The following command removes tags for the `mydata` bucket on the
`myminio` MinIO deployment:

```
mc tag remove myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] tag remove                \
                 [--rewind "string"]       \
                 [--versions]              \
                 [--version-id "string"]*  \
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

[`mc tag remove --version-id`](#mc.tag.remove.-version-id) is mutually exclusive with
multiple parameters. See the reference documentation for more information.

### Parameters

ALIAS[](#mc.tag.remove.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] for a MinIO deployment and the
full path to the object on which to remove all tags (e.g. bucket and path to
object). For example:

```
mc tag remove myminio/mybucket/object.txt
```

--recursive, r[](#mc.tag.remove.-recursive)
Optional

New in version RELEASE.2023-05-04T18-10-16Z.

Recursively removes all tags from all objects at the specified [`ALIAS`](#mc.tag.remove.ALIAS).

--rewind[](#mc.tag.remove.-rewind)
Optional

Directs [`mc tag remove`](#command-mc.tag.remove) to operate only on the object version(s) that
existed at specified point-in-time.

To rewind to a specific date in the past, specify the date as an
ISO8601-formatted timestamp. For example: `--rewind "2020.03.24T10:00"`.

To rewind a duration in time, specify the duration as a string in
`#d#hh#mm#ss` format. For example: `--rewind "1d2hh3mm4ss"`.

[`--rewind`](#mc.tag.remove.-rewind) requires that the specified [`ALIAS`](#mc.tag.remove.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

--versions[](#mc.tag.remove.-versions)
Optional

Directs [`mc tag remove`](#command-mc.tag.remove) to operate on all object versions that exist in the
bucket.

[`--versions`](#mc.tag.remove.-versions) requires that the specified [`ALIAS`](#mc.tag.remove.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Use [`--versions`](#mc.tag.remove.-versions) and
[`--rewind`](#mc.tag.remove.-rewind) together to remove tags from all
object versions which existed at a specific point in time.

--version-id, vid[](#mc.tag.remove.-version-id)
Optional

Directs [`mc tag remove`](#command-mc.tag.remove) to operate only on the specified object version.

[`--version-id`](#mc.tag.remove.-version-id) requires that the specified [`ALIAS`](#mc.tag.remove.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Mutually exclusive with the following parameters:

[`--rewind`](#mc.tag.remove.-rewind)

[`--versions`](#mc.tag.remove.-versions)

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Remove Tags from a Bucket or Object

Use [`mc tag remove`](#command-mc.tag.remove) to remove tags from a bucket or object:

```
mc tag remove ALIAS/PATH
```

Replace [`ALIAS`](#mc.tag.remove.ALIAS) with the
[[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

Replace [`PATH`](#mc.tag.remove.ALIAS) with the path to the bucket
or object on the MinIO deployment.

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