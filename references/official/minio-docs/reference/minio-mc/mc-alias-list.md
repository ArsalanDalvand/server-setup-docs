---
title: "mc alias list — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-alias-list.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-alias-list"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc alias list — SILO (MinIO Object Storage)"
---
# mc alias list

## Syntax

The [`mc alias list`](#command-mc.alias.list) command lists all aliases in the local
mc configuration.

The command output includes the configured access key and secret key associated
to each alias.

EXAMPLE

The following command lists all [[reference/minio-mc/mc-alias-set#alias|aliases]] configured
on the local host machine:

```
mc alias list
```

SYNTAX

The [`mc alias list`](#command-mc.alias.list) command has the following syntax:

```
mc [GLOBALFLAGS] alias list [ALIAS]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.alias.list.ALIAS)

Optional The name of a specific alias to display.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### List All Configured Aliases

Example

The following [`mc alias list`](#command-mc.alias.list) command lists all configured aliases
in the local mc configuration.

```
mc alias list
```

Syntax

```
mc alias list
```

### List a Specific Alias

Example

The following [`mc alias list`](#command-mc.alias.list) command lists the details of a
specific alias in the local mc configuration.

```
mc alias list myminio
```

Syntax

```
mc alias list ALIAS
```

Replace `ALIAS` with the the name of the alias to return.

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