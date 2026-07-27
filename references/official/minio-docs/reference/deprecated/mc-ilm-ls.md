---
title: "mc ilm ls — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-ilm-ls.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-ilm-ls"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm ls — SILO (MinIO Object Storage)"
---
# mc ilm ls

Changed in version RELEASE.2022-12-24T15-21-38Z: `mc ilm ls` replaced by [[reference/minio-mc/mc-ilm-rule-ls#command-mc.ilm.rule.ls|`mc ilm rule ls`]].

## Syntax

The [`mc ilm ls`](#command-mc.ilm.ls) command summrizes all configured object lifecycle management
rules on a MinIO bucket in a tabular format.

The output of the command might resemble the following:

```
┌───────────────────────────────────────────────────────────────────────────────┐
│ Transition for latest version (Transition)                                    │
├────────┬─────────┬────────┬─────────────────────┬──────────────┬──────────────┤
│ ID     │ STATUS  │ PREFIX │ TAGS                │ DAYS TO TIER │ TIER         │
├────────┼─────────┼────────┼─────────────────────┼──────────────┼──────────────┤
│ rule-1 │ Enabled │ doc/   │ key1=val1&key2=val2 │            0 │ WARM-MINIO-1 │
└────────┴─────────┴────────┴─────────────────────┴──────────────┴──────────────┘
┌────────────────────────────────────────────────────────────────┐
│ Transition for older versions (NoncurrentVersionTransition)    │
├────────┬─────────┬────────┬──────┬──────────────┬──────────────┤
│ ID     │ STATUS  │ PREFIX │ TAGS │ DAYS TO TIER │ TIER         │
├────────┼─────────┼────────┼──────┼──────────────┼──────────────┤
│ rule-2 │ Enabled │ logs/  │ -    │           10 │ WARM-MINIO-1 │
└────────┴─────────┴────────┴──────┴──────────────┴──────────────┘
┌────────────────────────────────────────────────────────────────────────────────────────┐
│ Expiration for latest version (Expiration)                                             │
├────────┬─────────┬────────┬─────────────────────┬────────────────┬─────────────────────┤
│ ID     │ STATUS  │ PREFIX │ TAGS                │ DAYS TO EXPIRE │ EXPIRE DELETEMARKER │
├────────┼─────────┼────────┼─────────────────────┼────────────────┼─────────────────────┤
│ rule-1 │ Enabled │ doc/   │ key1=val1&key2=val2 │             30 │ false               │
└────────┴─────────┴────────┴─────────────────────┴────────────────┴─────────────────────┘
┌──────────────────────────────────────────────────────────────────────────────────┐
│ Expiration for older versions (NoncurrentVersionExpiration)                      │
├────────┬─────────┬────────┬─────────────────────┬────────────────┬───────────────┤
│ ID     │ STATUS  │ PREFIX │ TAGS                │ DAYS TO EXPIRE │ KEEP VERSIONS │
├────────┼─────────┼────────┼─────────────────────┼────────────────┼───────────────┤
│ rule-1 │ Enabled │ doc/   │ key1=val1&key2=val2 │             15 │             0 │
│ rule-2 │ Enabled │ logs/  │ -                   │              1 │             3 │
└────────┴─────────┴────────┴─────────────────────┴────────────────┴───────────────┘
```

EXAMPLE

The following command lists all lifecycle management rules for the
`mydata` bucket on the `myminio` MinIO deployment:

```
mc ilm ls myminio/mydata
```

SYNTAX

The [`mc ilm ls`](#command-mc.ilm.ls) command has the following syntax:

```
mc [GLOBALFLAGS] ilm ls                        \
                 [--expiry | --transition]     \
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.ilm.ls.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] and full path to the bucket on the MinIO
deployment for which to list the object lifecycle management rules. For
example:

```
mc ilm ls myminio/mydata
```

--expiry[](#mc.ilm.ls.-expiry)
Optional

[`mc ilm ls`](#command-mc.ilm.ls) returns only fields related to lifecycle rule expiration.

Mutually exclusive with [`--transition`](#mc.ilm.ls.-transition).

--transition[](#mc.ilm.ls.-transition)
Optional

[`mc ilm ls`](#command-mc.ilm.ls) returns only fields related to lifecycle rule transition.

Mutually exclusive with [`--expiry`](#mc.ilm.ls.-expiry).

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### List Bucket Lifecycle Management Rules

Use [`mc ilm ls`](#command-mc.ilm.ls) to list a bucket’s lifecycle management rules:

```
mc ilm ls ALIAS/PATH
```

Replace [`ALIAS`](#mc.ilm.ls.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace [`PATH`](#mc.ilm.ls.ALIAS) with the path to the bucket on the
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