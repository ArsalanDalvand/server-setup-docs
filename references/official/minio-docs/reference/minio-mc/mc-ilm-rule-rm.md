---
title: "mc ilm rule rm — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-ilm-rule-rm.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-ilm-rule-rm"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm rule rm — SILO (MinIO Object Storage)"
---
# mc ilm rule rm

Changed in version RELEASE.2022-12-24T15-21-38Z: `mc ilm rule rm` replaces `mc ilm rm`.

## Syntax

The [`mc ilm rule rm`](#command-mc.ilm.rule.rm) command removes an object lifecycle management rule from a MinIO Bucket.

EXAMPLE

The following command removes a single lifecycle management rule from the `mydata` bucket on the `myminio` MinIO deployment:

```
mc ilm rule rm --id "bgrt1ghju" myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] ilm rule rm                         \
                     --id "string" | (--all --force) \
                     ALIAS                           \
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.ilm.rule.rm.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] and full path to the bucket on the MinIO deployment to which to remove the object lifecycle management rule.
For example:

```
mc ilm rule rm myminio/mydata
```

--all[](#mc.ilm.rule.rm.-all)
Optional

Removes all rules in the bucket.
Requires including [`--force`](#mc.ilm.rule.rm.-force).

Mutually exclusive with [`--id`](#mc.ilm.rule.rm.-id).

--force[](#mc.ilm.rule.rm.-force)
Optional

Required if specifying [`--all`](#mc.ilm.rule.rm.-all).

--id[](#mc.ilm.rule.rm.-id)
Optional

The unique ID of the rule.
Use [[reference/minio-mc/mc-ilm-rule-ls#command-mc.ilm.rule.ls|`mc ilm rule ls`]] to list bucket rules and retrieve the `id` for the rule you want to remove.

Mutually exclusive with [`mc ilm rule rm --all`](#mc.ilm.rule.rm.-all)

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Remove a Bucket Lifecycle Management Rule

Use [`mc ilm rule rm`](#command-mc.ilm.rule.rm) to remove a bucket lifecycle management rule:

```
mc ilm rule rm --id "RULE" ALIAS/PATH
```

Replace `RULE` with the unique identifier of the lifecycle  management rule.
Use [[reference/minio-mc/mc-ilm-rule-ls#command-mc.ilm.rule.ls|`mc ilm rule ls`]] to find the ID to use.

Replace [`ALIAS`](#mc.ilm.rule.rm.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace `PATH` with the path to the bucket on the S3-compatible host.

## Required Permissions

For permissions required to remove a rule, refer to the [[reference/minio-mc/mc-ilm-rule#minio-mc-ilm-rule-permissions|required permissions]] on the parent command.

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