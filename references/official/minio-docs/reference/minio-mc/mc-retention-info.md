---
title: "mc retention info — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-retention-info.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-retention-info"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc retention info — SILO (MinIO Object Storage)"
---
# mc retention info

## Syntax

The [`mc retention info`](#command-mc.retention.info) command configures the [[administration/object-management/object-retention#minio-object-locking|Write-Once Read-Many (WORM)
locking]] settings for an object or object(s) in a bucket.
You can also set the default object lock settings for a bucket, where all
objects without explicit object lock settings inherit the bucket default.

To lock an object under [[administration/object-management/object-retention#minio-object-locking-legalhold|legal hold]],
use [[reference/minio-mc/mc-legalhold-set#command-mc.legalhold.set|`mc legalhold set`]].

[`mc retention info`](#command-mc.retention.info) requires that the specified bucket has object locking
enabled. You can only enable object locking at bucket creation. See
[[reference/minio-mc/mc-mb#mc.mb.-with-lock|`mc mb --with-lock`]] for documentation on creating buckets with
object locking enabled.

EXAMPLE

The following command returns the default object lock configuration for
the `mydata` bucket on the `myminio` MinIO deployment:

```
mc retention info --default myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] retention info            \
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

[`mc retention info --version-id`](#mc.retention.info.-version-id) is mutually exclusive with
multiple other parameters. See the reference documentation for more
information.

### Parameters

ALIAS[](#mc.retention.info.ALIAS)
Required

The full path to the object for which to retrieve
the object lock configuration. Specify the [[reference/minio-mc/mc-alias-set#alias|alias]] of a
configured S3-compatible service as the prefix to the `ALIAS` bucket
path. For example:

```
mc retention info play/mybucket/object.txt
```

If the `ALIAS` specifies a bucket or bucket prefix, include

[`--recursive`](#mc.retention.info.-recursive) to return the object
lock settings for all objects in the bucket or bucket prefix.

If the `ALIAS` bucket has versioning enabled,

[`mc retention info`](#command-mc.retention.info) by default applies to only the latest object
version. Use [`--version-id`](#mc.retention.info.-version-id) or
[`--versions`](#mc.retention.info.-versions) to return the object lock
settings for a specific version or for all versions of the object.

--default[](#mc.retention.info.-default)
Optional

Returns the default object lock settings for the bucket specified
to [`ALIAS`](#mc.retention.info.ALIAS).

If specifying [`--default`](#mc.retention.info.-default),
[`mc retention info`](#command-mc.retention.info) ignores all other flags.

--recursive, r[](#mc.retention.info.-recursive)
Optional

Recursively returns the object lock settings for all objects in the
specified [`ALIAS`](#mc.retention.info.ALIAS) path.

Mutually exclusive with [`--version-id`](#mc.retention.info.-version-id).

--rewind[](#mc.retention.info.-rewind)
Optional

Directs [`mc retention info`](#command-mc.retention.info) to operate only on the object version(s) that
existed at specified point-in-time.

To rewind to a specific date in the past, specify the date as an
ISO8601-formatted timestamp. For example: `--rewind "2020.03.24T10:00"`.

To rewind a duration in time, specify the duration as a string in
`#d#hh#mm#ss` format. For example: `--rewind "1d2hh3mm4ss"`.

[`--rewind`](#mc.retention.info.-rewind) requires that the specified [`ALIAS`](#mc.retention.info.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

--version-id, vid[](#mc.retention.info.-version-id)
Optional

Directs [`mc retention info`](#command-mc.retention.info) to operate only on the specified object version.

[`--version-id`](#mc.retention.info.-version-id) requires that the specified [`ALIAS`](#mc.retention.info.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Mutually exclusive with any of the following flags:

[`--versions`](#mc.retention.info.-versions)

[`--rewind`](#mc.retention.info.-rewind)

[`--recursive`](#mc.retention.info.-recursive)

--versions[](#mc.retention.info.-versions)
Optional

Directs [`mc retention info`](#command-mc.retention.info) to operate on all object versions that exist in the
bucket.

[`--versions`](#mc.retention.info.-versions) requires that the specified [`ALIAS`](#mc.retention.info.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Use [`--versions`](#mc.retention.info.-versions) and
[`--rewind`](#mc.retention.info.-rewind) together to retrieve the
retention settings for all object versions that existed at a
specific point-in-time.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Retrieve Object Lock Settings for an Object or Object(s)

Specific Object

```
mc retention info ALIAS/PATH
```

Replace [`ALIAS`](#mc.retention.info.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`PATH`](#mc.retention.info.ALIAS) with the path to the
object.

Multiple Objects

Use [`mc retention info`](#command-mc.retention.info) with
[`--recursive`](#mc.retention.info.-recursive) to retrieve the retention
settings for all objects in a bucket:

```
mc retention info --recursive ALIAS/PATH
```

Replace [`ALIAS`](#mc.retention.info.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`PATH`](#mc.retention.info.ALIAS) with the path to the
bucket.

The bucket must have object locking enabled to use this command.
You can only enable object locking when creating a bucket. See
[[reference/minio-mc/mc-mb#mc.mb.-with-lock|`mc mb --with-lock`]] for more information on creating buckets
with object locking enabled.

### Retrieve Default Object Lock Settings for a Bucket

Use [`mc retention info`](#command-mc.retention.info) with
[`--default`](#mc.retention.info.-default) to retrieve the default
object lock settings for a bucket:

```
mc retention info --default ALIAS/PATH
```

Replace [`ALIAS`](#mc.retention.info.ALIAS) with the

[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`PATH`](#mc.retention.info.ALIAS) with the path to the
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