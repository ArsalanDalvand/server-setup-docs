---
title: "mc ilm rule ls — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-ilm-rule-ls.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-ilm-rule-ls"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm rule ls — SILO (MinIO Object Storage)"
---
# mc ilm rule ls

Changed in version RELEASE.2022-12-24T15-21-38Z: `mc ilm rule ls` replaces `mc ilm ls`.

Changed in version RELEASE.2023-05-26T23-31-54Z: `mc ilm rule ls --json` output includes the policy modification time in `updateAt`.

## Syntax

The [`mc ilm rule ls`](#command-mc.ilm.rule.ls) command summarizes all configured object lifecycle management rules on a MinIO bucket in a tabular format.

The [`mc ilm rule list`](#command-mc.ilm.rule.list) command has equivalent functionality to [`mc ilm rule ls`](#command-mc.ilm.rule.ls).

EXAMPLE

The following command lists all lifecycle management rules for the `mydata` bucket on the `myminio` MinIO deployment:

```
mc ilm rule ls myminio/mydata
```

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

SYNTAX

The [`mc ilm rule ls`](#command-mc.ilm.rule.ls) command has the following syntax:

```
mc [GLOBALFLAGS] ilm rule ls     \
                 [--expiry]      \
                 [--transition]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.ilm.rule.ls.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] and full path to the bucket on the MinIO deployment for which to list the object lifecycle management rules.
For example:

```
mc ilm rule ls myminio/mydata
```

--expiry[](#mc.ilm.rule.ls.-expiry)
Optional

[`mc ilm rule ls`](#command-mc.ilm.rule.ls) returns only fields related to lifecycle rule expiration.

Mutually exclusive with [`--transition`](#mc.ilm.rule.ls.-transition).

--transition[](#mc.ilm.rule.ls.-transition)
Optional

[`mc ilm rule ls`](#command-mc.ilm.rule.ls) returns only fields related to lifecycle rule transition.

Mutually exclusive with [`--expiry`](#mc.ilm.rule.ls.-expiry).

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### List Bucket Lifecycle Management Rules

Use [`mc ilm rule ls`](#command-mc.ilm.rule.ls) to list a bucket’s lifecycle management rules:

```
mc ilm rule ls ALIAS/PATH
```

Replace [`ALIAS`](#mc.ilm.rule.ls.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace `PATH` with the path to the bucket on the S3-compatible host.

### Show Policy Modification Time

Use [`mc ilm rule ls`](#command-mc.ilm.rule.ls) with :option::–json <mc.–json> to show the time the policy for a bucket was last updated.

```
mc ilm rule ls ALIAS/PATH --json
```

Replace [`ALIAS`](#mc.ilm.rule.ls.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the S3-compatible host.

Replace `PATH` with the path to the bucket on the S3-compatible host.

The `updateAt` property in the JSON output contains the date and time the policy was updated.

The output resembles the following:

```
{
 "status": "success",
 "target": "myminio/mybucket",
 "config": {
  "Rules": [
   {
    "Expiration": {
     "Days": 30
    },
    "ID": "ci1o2mg0sko6f1r3krv0",
    "Status": "Enabled"
   }
  ]
 },
 "updatedAt": "2023-06-09T19:45:30Z"
}
```

## Required Permissions

For permissions required to list rules, refer to the [[reference/minio-mc/mc-ilm-rule#minio-mc-ilm-rule-permissions|required permissions]] on the parent command.

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