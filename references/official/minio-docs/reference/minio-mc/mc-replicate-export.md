---
title: "mc replicate export — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-replicate-export.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-replicate-export"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc replicate export — SILO (MinIO Object Storage)"
---
# mc replicate export

## Syntax

The [`mc replicate export`](#command-mc.replicate.export) command exports the JSON-formatted
[[administration/bucket-replication#minio-bucket-replication-serverside|replication rules]] for a
MinIO bucket to `STDOUT`.

EXAMPLE

The following command exports the replication configuration for the
`mydata` bucket on the `myminio` MinIO deployment:

```
mc replicate export myminio/mydata > mydata-replication.json
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] export ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.replicate.export.ALIAS)

Required the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment and full path to
the bucket or bucket prefix for which to export the replication rules. For
example:

```
mc replicate export myminio/mybucket
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Export Existing Replication Rules

Use [`mc replicate export`](#command-mc.replicate.export) to export bucket replication rules:

```
mc replicate export ALIAS/PATH > bucket-replication-rules.json
```

Replace [`ALIAS`](#mc.replicate.export.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`PATH`](#mc.replicate.export.ALIAS) with the path to the
bucket or bucket prefix.

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