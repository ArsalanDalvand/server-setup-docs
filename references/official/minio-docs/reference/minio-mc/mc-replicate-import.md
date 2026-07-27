---
title: "mc replicate import — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-replicate-import.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-replicate-import"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc replicate import — SILO (MinIO Object Storage)"
---
# mc replicate import

## Syntax

The [`mc replicate import`](#command-mc.replicate.import) command imports JSON-formatted
[[administration/bucket-replication#minio-bucket-replication-serverside|replication rules]] for a
MinIO bucket from `STDIN`.

EXAMPLE

The following command imports the replication configuration for the
`mydata` bucket on the `myminio` MinIO deployment:

```
mc replicate import myminio/mydata < mydata-replication.json
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] import ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.replicate.import.ALIAS)

Required the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment and full path to
the bucket or bucket prefix for which to import the replication rules. For
example:

```
mc replicate import myminio/mybucket
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Import Existing Replication Rules

Use [`mc replicate import`](#command-mc.replicate.import) to import bucket replication rules:

```
mc replicate import ALIAS/PATH < bucket-replication-rules.json
```

Replace [`ALIAS`](#mc.replicate.import.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`PATH`](#mc.replicate.import.ALIAS) with the path to the
bucket or bucket prefix.

## Behavior

### Importing Configuration Overrides Existing Rules

[`mc replicate import`](#command-mc.replicate.import) replaces the current bucket replication
rules with those defined in the imported JSON configuration.

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