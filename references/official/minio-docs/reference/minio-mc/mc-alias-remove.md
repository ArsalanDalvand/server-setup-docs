---
title: "mc alias remove — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-alias-remove.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-alias-remove"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc alias remove — SILO (MinIO Object Storage)"
---
# mc alias remove

## Syntax

The [`mc alias remove`](#command-mc.alias.remove) removes an existing alias from the local
mc configuration.

EXAMPLE

The following command removes the `myminio` [[reference/minio-mc/mc-alias-set#alias|alias]] for a
MinIO deployment from the host machine:

```
mc alias remove myminio
```

SYNTAX

The [`mc alias remove`](#command-mc.alias.remove) command has the following syntax:

```
mc [GLOBALFLAGS] alias remove ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.alias.remove.ALIAS)

Required The alias to remove from the local mc configuration.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Remove an Alias from the mc Configuration

Use [`mc alias remove`](#command-mc.alias.remove) to remove an existing alias from the
mc configuration:

Example

The following command removes the `myminio` alias.

```
mc alias remove myminio
```

Syntax

```
mc alias remove ALIAS
```

Replace `ALIAS` with the the name of the alias to remove.

### Behavior

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