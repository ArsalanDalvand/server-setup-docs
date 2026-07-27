---
title: "mc tree — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-tree.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-tree"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc tree — SILO (MinIO Object Storage)"
---
# mc tree

## Syntax

The [`mc tree`](#command-mc.tree) command lists all prefixes inside a MinIO bucket in a tree
format. The command optionally supports listing all objects inside of bucket
at each prefix, including the bucket root.

You can also use [`mc tree`](#command-mc.tree) against a local filesystem directory to
produce similar results to the `tree` commandline tool.

EXAMPLE

The following command prints a complete tree of all objects at any
depth in the `mydata` bucket on the `myminio` MinIO deployment:

```
mc tree --files myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] tree                 \
                 [--depth int]        \
                 [--files]            \
                 [--rewind "string"]  \
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.tree.ALIAS)

Required The [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment and the
full path to the bucket to list the tree hierarchy. For example:

```
mc tree myminio/mybucket
```

You can specify multiple targets to the [`mc tree`](#command-mc.tree) command. For
example:

```
mc tree myminio/mybucket myminio/myotherbucket
```

For retrieving the tree heirarchy of a local filesystem directory,
specify the full path to that directory. For example:

```
mc tree ~/minio/mydata/
```

--depth, d[](#mc.tree.-depth)

Optional Limit the tree depth to the specified integer value.

Defaults to `-1` or unlimited depth.

--files, f[](#mc.tree.-files)

Optional Includes files in the object or directory in the [`mc tree`](#command-mc.tree)
output.

--rewind[](#mc.tree.-rewind)
Optional

Directs [`mc tree`](#command-mc.tree) to operate only on the object version(s) that
existed at specified point-in-time.

To rewind to a specific date in the past, specify the date as an
ISO8601-formatted timestamp. For example: `--rewind "2020.03.24T10:00"`.

To rewind a duration in time, specify the duration as a string in
`#d#hh#mm#ss` format. For example: `--rewind "1d2hh3mm4ss"`.

[`--rewind`](#mc.tree.-rewind) requires that the specified [`ALIAS`](#mc.tree.ALIAS) be an S3-compatible service
that supports [[administration/object-management/object-versioning#minio-bucket-versioning|Bucket Versioning]]. For MinIO deployments, use
[[reference/minio-mc/mc-version#command-mc.version|`mc version`]] to enable or disable bucket versioning.

## Examples

```
mc tree ALIAS/PATH
```

Replace [`ALIAS`](#mc.tree.ALIAS) with the [[reference/minio-mc/mc-alias-set#alias|alias]]
of the MinIO deployment.

Replace [`PATH`](#mc.tree.ALIAS) with the path to the bucket on the
MinIO deployment.

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