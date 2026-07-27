---
title: "mc anonymous get — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-anonymous-get.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-anonymous-get"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc anonymous get — SILO (MinIO Object Storage)"
---
# mc anonymous get

## Syntax

The [`mc anonymous get`](#command-mc.anonymous.get) command gets the anonymous (i.e. unauthenticated or
public) access [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] for a bucket.

Buckets with anonymous policies allow clients to access the bucket contents
and perform actions consistent with the specified policy without
[[administration/identity-access-management#minio-authentication-and-identity-management|authentication]].

To get the [JSON policy](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-iam-policies) assigned to the bucket,
use the [[reference/minio-mc/mc-anonymous-get-json#command-mc.anonymous.get-json|`mc anonymous get-json`]] command.

EXAMPLE

The following command retrieves the anonymous access policy for the
`mydata` bucket on the `myminio` MinIO deployment:

```
mc anonymous get myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] policy get ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.anonymous.get.ALIAS)

Required The full path to the bucket or bucket prefix for which the
command retrieves the anonymous bucket policy.

Specify the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO or other
S3-compatible service and the full path to the bucket or bucket
prefix. For example:

```
mc anonymous get public play/mybucket
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Get Anonymous Policy for Bucket

Use [`mc anonymous get`](#command-mc.anonymous.get) to get the anonymous policy for a
bucket:

```
mc anonymous get ALIAS/PATH
```

Replace [`ALIAS`](#mc.anonymous.get.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`PATH`](#mc.anonymous.get.ALIAS) with the destination bucket.

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