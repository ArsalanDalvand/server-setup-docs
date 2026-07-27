---
title: "mc support top disk — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-support-top-disk.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-support-top-disk"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc support top disk — SILO (MinIO Object Storage)"
---
# mc support top disk

SUBNET Registration Required

The `mc support` commands are designed for MinIO deployments registered with [MinIO SUBNET](https://min.io/pricing?jmp=docs) to ensure optimal outcome of diagnostics and performance testing.
Deployments not registered with SUBNET cannot use the `mc support` commands.

## Syntax

The [`mc support top disk`](#command-mc.support.top.disk) command displays current drive statistics.

EXAMPLE

The following command displays the current in-progress S3 API calls on the [[glossary#term-alias|alias]] `myminio`.

```
mc support top disk myminio/
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] support top disk                     \
                             [--count, -c "integer"]  \
                             TARGET
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

TARGET[](#mc.support.top.disk.TARGET)
Required

The full path to the [[reference/minio-mc/mc-alias-set#minio-mc-alias|alias]] or [[glossary#term-prefix|prefix]] where the command should run.

--count, -c[](#mc.support.top.disk.-count)
Optional

Display statistics for up to the entered number of drives.

If no entry is made, the command returns statistics for up to 10 drives.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).