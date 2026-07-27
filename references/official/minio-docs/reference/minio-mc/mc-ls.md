---
title: "mc ls — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-ls.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-ls"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ls — SILO (MinIO Object Storage)"
---
# mc ls

## Syntax

The [`mc ls`](#command-mc.ls) command lists buckets and objects on MinIO or another
S3-compatible service.

You can also use [`mc ls`](#command-mc.ls) against the local filesystem to produce similar
results as the `ls` command.

EXAMPLE

The following command lists all objects and object versions in the
`mydata` bucket on the `myminio` MinIO deployment:

```
mc ls --recursive --versions myminio/mydata
```

The output resembles the following:

```
[2022-11-08 11:30:24 PST]    52MB  STANDARD log-data.csv
[2022-11-09 12:20:18 PST]    120MB WARM videos/event-2022-11-09.mp4
```

`STANDARD` marks objects stored on the MinIO deployment

`WARM` marks objects stored on the remote tier with matching name

`videos/` indicates the prefix for the object

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] ls              \
                 [--incomplete]  \
                 [--recursive]   \
                 [--rewind]      \
                 [--versions]    \
                 [--summarize]   \
                 ALIAS [ALIAS ...]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.ls.ALIAS)

Required The object or objects to copy.

For listing objects on MinIO,
specify the [[reference/minio-mc/mc-alias-set#alias|alias]] and the full path to that
object (e.g. bucket and path to object). For example:

```
mc ls play/mybucket/object.txt
```

For listing objects on a local filesystem, specify the full
path to that object. For example:

```
mc ls ~/mydata/object.txt
```

If you specify a directory or bucket to [`ALIAS`](#mc.ls.ALIAS), you must
also specify [`--recursive`](#mc.ls.-recursive) to recursively list the
contents of that directory or bucket. If you omit the `--recursive`
argument, [`ls`](#command-mc.ls) only lists objects in the top level of the specified
directory or bucket.

incomplete, -I[](#mc.ls.incomplete)

Optional Returns any incomplete uploads on the specified
[`ALIAS`](#mc.ls.ALIAS) bucket.

--recursive, r[](#mc.ls.-recursive)

Optional Recursively lists the contents of each bucket or directory in the
[`ALIAS`](#mc.ls.ALIAS).

--rewind[](#mc.ls.-rewind)
Optional

Directs [`mc ls`](#command-mc.ls) to operate only on the object version(s) that
existed at specified point-in-time.

To rewind to a specific date in the past, specify the date as an
ISO8601-formatted timestamp. For example: `--rewind "2020.03.24T10:00"`.

To rewind a duration in time, specify the duration as a string in
`#d#hh#mm#ss` format. For example: `--rewind "1d2hh3mm4ss"`.

[`--rewind`](#mc.ls.-rewind) requires that the specified [`ALIAS`](#mc.ls.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Use [`--rewind`](#mc.ls.-rewind) and
[`--versions`](#mc.ls.-versions) together to display on those object
versions which existed at a specific point in time.

--versions[](#mc.ls.-versions)
Optional

Directs [`mc ls`](#command-mc.ls) to operate on all object versions that exist in the
bucket.

[`--versions`](#mc.ls.-versions) requires that the specified [`ALIAS`](#mc.ls.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

Use [`--versions`](#mc.ls.-versions) and
[`--rewind`](#mc.ls.-rewind) together to display on those object
versions which existed at a specific point in time.

--summarize[](#mc.ls.-summarize)

Optional Displays summarized information for the specified `ALIAS` path.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### List Bucket Contents

Use [`mc ls`](#mc.ls.ALIAS) to list the contents of a bucket:

```
mc ls [--recursive] ALIAS/PATH
```

Replace [`ALIAS`](#mc.ls.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`PATH`](#mc.ls.ALIAS) with the path to the bucket on the
S3-compatible host.

If specifying the path to the S3 root (`ALIAS` only), include the
[`--recursive`](#mc.ls.-recursive) option.

### List Object Versions

Use [`mc ls --versions`](#mc.ls.-versions) to list all versions of an object:

```
mc ls --versions ALIAS/PATH
```

Replace [`ALIAS`](#mc.ls.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`PATH`](#mc.ls.ALIAS) with the path to the bucket or object on
the S3-compatible host.

Requires Versioning

[`mc ls`](#command-mc.ls) requires [[administration/object-management/object-versioning#minio-bucket-versioning|bucket versioning]] to
use this feature. Use [[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable versioning on a bucket.

### List Bucket Contents at Point in Time

Use [`mc ls --versions`](#mc.ls.-versions) to list all versions of an object:

```
mc ls --rewind DURATION ALIAS/PATH
```

Replace [`ALIAS`](#mc.ls.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`PATH`](#mc.ls.ALIAS) with the path to the bucket or object on
the S3-compatible host.

Replace [`DURATION`](#mc.ls.-rewind) with the point-in-time in the past
at which the command returns the object. For example, specify `30d` to
return the version of the object 30 days prior to the current date.

Requires Versioning

[`mc ls`](#command-mc.ls) requires [[administration/object-management/object-versioning#minio-bucket-versioning|bucket versioning]] to
use this feature. Use [[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable versioning on a bucket.

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