---
title: "mc ilm export — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-ilm-export.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-ilm-export"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm export — SILO (MinIO Object Storage)"
---
# mc ilm export

Changed in version RELEASE.2022-12-24T15-21-38Z: `mc ilm export` replaced by [[reference/minio-mc/mc-ilm-rule-export#command-mc.ilm.rule.export|`mc ilm rule export`]].

## Syntax

The [`mc ilm export`](#command-mc.ilm.export) command exports the object lifecycle management
configuration for a MinIO bucket.

The [`mc ilm export`](#command-mc.ilm.export) command outputs to `STDOUT` by default. You can
output the contents to a `.json` file for archival or ingestion using
[[reference/deprecated/mc-ilm-import#command-mc.ilm.import|`mc ilm import`]].

EXAMPLE

The following command exports the lifecycle management configuration
of the `mydata` bucket on the `myminio` deployment to the
`mydata-lifecycle-config.json` file:

```
mc ilm export myminio/mydata > mydata-lifecycle-config.json
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] ilm export ALIAS > STDOUT
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.ilm.export.ALIAS)

Required The [[reference/minio-mc/mc-alias-set#alias|alias]] and full path to the bucket on the MinIO
deployment for which to export object lifecycle management rules. For
example:

```
mc ilm export myminio/mydata > bucket-lifecycle.json
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Export the Bucket Lifecycle Management Configuration

Example

The following command exports the bucket lifecycle management
configuration to the `bucket-lifecycle.json` file:

```
mc ilm export myminio/mybucket > bucket-lifecycle.json
```

Syntax

```
mc ilm export ALIAS > file.json
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO
deployment and the bucket for which to export object lifecycle
management rules:

`myminio/mydata`

Replace `file.json` with the name of the file to which to export the
lifecycle management rules.

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