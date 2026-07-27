---
title: "mc ilm rule import — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-ilm-rule-import.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-ilm-rule-import"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm rule import — SILO (MinIO Object Storage)"
---
# mc ilm rule import

Changed in version RELEASE.2022-12-24T15-21-38Z: `mc ilm rule import` replaces `mc ilm import`.

## Syntax

The [`mc ilm rule import`](#command-mc.ilm.rule.import) command imports an object lifecycle management
configuration and applies it to a MinIO bucket.

The [`mc ilm rule import`](#command-mc.ilm.rule.import) command imports from `STDIN` by default.
You can input the contents from a `.json` file, such as one produced by [[reference/minio-mc/mc-ilm-rule-export#command-mc.ilm.rule.export|`mc ilm rule export`]].

EXAMPLE

The following command imports the lifecycle management configuration from
`mydata-lifecycle-config.json` and applies it to the `mydata` bucket on the `myminio` deployment:

```
mc ilm rule import myminio/mydata < mydata-lifecycle-config.json
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] ilm rule import ALIAS < STDIN
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.ilm.rule.import.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] and full path to the bucket on the MinIO deployment into which to import object lifecycle management rules.
For example:

```
mc ilm rule import myminio/mydata < bucket-lifecycle.json
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Import the Bucket Lifecycle Management Configuration

Example

The following command imports the bucket lifecycle management
configuration from the `bucket-lifecycle.json` file:

```
mc ilm rule import myminio/mybucket < bucket-lifecycle.json
```

Syntax

```
mc ilm rule import ALIAS < file.json
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO
deployment and the bucket into which to import object lifecycle
management rules:

`myminio/mydata`

Replace `file.json` with the name of the file from which to import the
lifecycle management rules.

## Required Permissions

For permissions required to import rules, refer to the [[reference/minio-mc/mc-ilm-rule#minio-mc-ilm-rule-permissions|required permissions]] on the parent command.

## Behavior

### Importing Configuration Overrides Existing Rules

[`mc ilm rule import`](#command-mc.ilm.rule.import) replaces the current bucket lifecycle management
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