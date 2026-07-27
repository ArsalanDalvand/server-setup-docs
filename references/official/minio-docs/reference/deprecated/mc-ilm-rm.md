---
title: "mc ilm rm — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-ilm-rm.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-ilm-rm"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm rm — SILO (MinIO Object Storage)"
---
# mc ilm rm

Changed in version RELEASE.2022-12-24T15-21-38Z: `mc ilm rm` replaced by [[reference/minio-mc/mc-ilm-rule-rm#command-mc.ilm.rule.rm|`mc ilm rule rm`]].

## Syntax

The [`mc ilm rm`](#command-mc.ilm.rm) command removes an object lifecycle management rule from a
MinIO Bucket.

The [`mc ilm remove`](#command-mc.ilm.remove) command has equivalent functionality to [`mc ilm rm`](#command-mc.ilm.rm).

EXAMPLE

The following command removes a single lifecycle management rule from
the `mydata` bucket on the `myminio` MinIO deployment:

```
mc ilm rm --id "bgrt1ghju" myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] ilm rm                          \
                 --id "string" | (--all --force) \
                 ALIAS                           \
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.ilm.rm.ALIAS)

Required The [[reference/minio-mc/mc-alias-set#alias|alias]] and full path to the bucket on the MinIO
deployment to which to remove the object lifecycle management rule. For
example:

```
mc ilm rm myminio/mydata
```

all[](#mc.ilm.rm.all)

Required Removes all rules in the bucket. Mutually exclusive with
[`mc ilm rm id`](#mc.ilm.rm.id).

Mutually exclusive with [`mc ilm rm id`](#mc.ilm.rm.id)

Requires including [`force`](#mc.ilm.rm.force).

force[](#mc.ilm.rm.force)

Required if specifying [`all`](#mc.ilm.rm.all).

id[](#mc.ilm.rm.id)

Required The unique ID of the rule. Use [[reference/minio-mc/mc-ilm-rule-ls#command-mc.ilm.rule.ls|`mc ilm rule ls`]] to list bucket
rules and retrieve the `id` for the rule you want to remove.

Mutually exclusive with [`mc ilm rm all`](#mc.ilm.rm.all)

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Remove a Bucket Lifecycle Management Rule

Use [`mc ilm rm`](#command-mc.ilm.rm) to remove a bucket lifecycle management rule:

```
mc ilm rm --id "RULE" ALIAS/PATH
```

Replace [`RULE`](#mc.ilm.rm.id) with the unique name of the lifecycle
management rule.

Replace [`ALIAS`](#mc.ilm.rm.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`PATH`](#mc.ilm.rm.ALIAS) with the path to the bucket on the
S3-compatible host.

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