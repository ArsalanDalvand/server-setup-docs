---
title: "mc replicate ls — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-replicate-ls.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-replicate-ls"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc replicate ls — SILO (MinIO Object Storage)"
---
# mc replicate ls

Changed in version RELEASE.2022-12-24T15-21-38Z: `mc replicate ls` replaces the `mc admin bucket remote ls` command.

## Syntax

The [`mc replicate ls`](#command-mc.replicate.ls) command lists all
[[administration/bucket-replication#minio-bucket-replication-serverside|replication rules]] on a
MinIO bucket.

The [`mc replicate list`](#command-mc.replicate.list) command has equivalent functionality to [`mc replicate ls`](#command-mc.replicate.ls).

EXAMPLE

The following command lists all enabled replication rules for the
`mydata` bucket on the `myminio` MinIO deployment:

```
mc replicate ls --status "enabled" myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] replicate ls         \
                 [--status "string"]  \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.replicate.ls.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment and full path to
the bucket or bucket prefix for which to list the replication rules. For
example:

```
mc replicate ls myminio/mybucket
```

--status[](#mc.replicate.ls.-status)
Optional

Filter replication rules on the bucket based on their status.
Specify one of the following values:

`enabled` - Show only enabled replication rules.

`disabled` - Show only disabled replication rules.

If omitted, [`mc replicate ls`](#command-mc.replicate.ls) defaults to showing all replication
rules.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### List Existing Replication Rules

Use [`mc replicate ls`](#command-mc.replicate.ls) to list bucket replication rules:

```
mc replicate ls ALIAS/PATH
```

Replace [`ALIAS`](#mc.replicate.ls.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`PATH`](#mc.replicate.ls.ALIAS) with the path to the
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