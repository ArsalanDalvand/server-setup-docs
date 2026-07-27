---
title: "mc anonymous get-json — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-anonymous-get-json.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-anonymous-get-json"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc anonymous get-json — SILO (MinIO Object Storage)"
---
# mc anonymous get-json

## Syntax

The [`mc anonymous get-json`](#command-mc.anonymous.get-json) command gets anonymous (i.e. unauthenticated or
public) access [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] for a bucket.

Buckets with anonymous policies allow clients to access the bucket contents
and perform actions consistent with the specified policy without
[[administration/identity-access-management#minio-authentication-and-identity-management|authentication]].

EXAMPLE

The following command retrieves the JSON-formatted anonymous
policy for the `mydata` bucket on the `myminio` MinIO deployment:

```
mc anonymous get-json myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] get-json ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.anonymous.get-json.ALIAS)

Required The full path to the bucket or bucket prefix for which to get the
anonymous bucket policy.

Specify the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO or other
S3-compatible service and the full path to the bucket or bucket
prefix. For example:

```
mc anonymous get-json public play/mybucket
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Get Anonymous Policy for Bucket

Use [`mc anonymous get-json`](#command-mc.anonymous.get-json) to get the anonymous policy for a
bucket:

```
mc anonymous get-json ALIAS/PATH
```

Replace [`ALIAS`](#mc.anonymous.get-json.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`PATH`](#mc.anonymous.get-json.ALIAS) with the destination bucket.

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