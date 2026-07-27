---
title: "mc retention clear — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-retention-clear.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-retention-clear"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc retention clear — SILO (MinIO Object Storage)"
---
# mc retention clear

## Syntax

The [`mc retention clear`](#command-mc.retention.clear) command removes the
[[administration/object-management/object-retention#minio-object-locking|Write-Once Read-Many (WORM) locking]] settings for
an object or object(s) in a bucket. You can also remove the default object lock
settings for a bucket.

To change the retention status of an object under
[[administration/object-management/object-retention#minio-object-locking-legalhold|legal hold]], use
[[reference/minio-mc/mc-legalhold-clear#command-mc.legalhold.clear|`mc legalhold clear`]].

EXAMPLE

The following command removes the default object lock configuration for
the `mydata` bucket on the `myminio` MinIO deployment:

```
mc retention clear --default myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] retention clear           \
                 [--default]               \
                 [--recursive]             \
                 [--rewind "string"]       \
                 [--version-id "string"]*  \
                 [--versions]              \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

[`mc retention clear --version-id`](#mc.retention.clear.-version-id) is mutually exclusive with
multiple other parameters. See the reference documentation for more
information.

### Parameters

ALIAS[](#mc.retention.clear.ALIAS)
Required

The full path to the object or objects for which to clear
the object lock configuration. Specify the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a
configured S3-compatible service as the prefix to the `ALIAS` bucket
path. For example:

```
mc retention clear play/mybucket/object.txt
```

If the `ALIAS` specifies a bucket or bucket prefix, include

[`--recursive`](#mc.retention.clear.-recursive) to clear the object lock
settings to the bucket contents.

If the `ALIAS` bucket has versioning enabled,

[`mc retention clear`](#command-mc.retention.clear) by default applies to only the latest
object version. Use [`--version-id`](#mc.retention.clear.-version-id) or
[`--versions`](#mc.retention.clear.-versions) to clear the object lock
settings for a specific version or for all versions of the object.

--default[](#mc.retention.clear.-default)
Optional

Clears the default object lock settings for the bucket specified
to [`ALIAS`](#mc.retention.clear.ALIAS).

If specifying [`--default`](#mc.retention.clear.-default),
[`mc retention clear`](#command-mc.retention.clear) ignores all other flags.

--recursive, r[](#mc.retention.clear.-recursive)
Optional

Recursively clears the object lock settings for all objects in the
specified [`ALIAS`](#mc.retention.clear.ALIAS) path.

Mutually exclusive with [`--version-id`](#mc.retention.clear.-version-id).

--rewind[](#mc.retention.clear.-rewind)
Optional

Directs [`mc retention clear`](#command-mc.retention.clear) to operate only on the object version(s) that
existed at specified point-in-time.

To rewind to a specific date in the past, specify the date as an
ISO8601-formatted timestamp. For example: `--rewind "2020.03.24T10:00"`.

To rewind a duration in time, specify the duration as a string in
`#d#hh#mm#ss` format. For example: `--rewind "1d2hh3mm4ss"`.

[`--rewind`](#mc.retention.clear.-rewind) requires that the specified [`ALIAS`](#mc.retention.clear.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

--version-id, vid[](#mc.retention.clear.-version-id)
Optional

Directs [`mc retention clear`](#command-mc.retention.clear) to operate only on the specified object version.

[`--version-id`](#mc.retention.clear.-version-id) requires that the specified [`ALIAS`](#mc.retention.clear.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Mutually exclusive with any of the following flags:

[`--versions`](#mc.retention.clear.-versions)

[`--rewind`](#mc.retention.clear.-rewind)

[`--recursive`](#mc.retention.clear.-recursive)

--versions[](#mc.retention.clear.-versions)
Optional

Directs [`mc retention clear`](#command-mc.retention.clear) to operate on all object versions that exist in the
bucket.

[`--versions`](#mc.retention.clear.-versions) requires that the specified [`ALIAS`](#mc.retention.clear.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Use [`--versions`](#mc.retention.clear.-versions) and
[`--rewind`](#mc.retention.clear.-rewind) together to remove the
retention settings from all object versions that existed at a
specific point-in-time.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Clear Object Lock Settings for an Object or Object(s)

Specific Object

```
mc retention clear ALIAS/PATH
```

Replace [`ALIAS`](#mc.retention.clear.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`PATH`](#mc.retention.clear.ALIAS) with the path to the
object.

Multiple Objects

Use [`mc retention clear`](#command-mc.retention.clear) with
[`--recursive`](#mc.retention.clear.-recursive) to clear the retention
settings from all objects in a bucket:

```
mc retention clear --recursive ALIAS/PATH
```

Replace [`ALIAS`](#mc.retention.clear.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`PATH`](#mc.retention.clear.ALIAS) with the path to the
bucket.

The bucket must have object locking enabled to use this command.
You can only enable object locking when creating a bucket. See
[[reference/minio-mc/mc-mb#mc.mb.-with-lock|`mc mb --with-lock`]] for more information on creating buckets
with object locking enabled.

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