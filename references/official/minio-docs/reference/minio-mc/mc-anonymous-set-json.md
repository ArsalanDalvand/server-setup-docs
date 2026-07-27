---
title: "mc anonymous set-json — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-anonymous-set-json.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-anonymous-set-json"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc anonymous set-json — SILO (MinIO Object Storage)"
---
# mc anonymous set-json

## Syntax

The [`mc anonymous set-json`](#command-mc.anonymous.set-json) command sets anonymous (i.e. unauthenticated or
public) access [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] for a bucket using using an IAM
[JSON policy document](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-iam-policies).

Buckets with anonymous policies allow clients to access the bucket contents
and perform actions consistent with the specified policy without
[[administration/identity-access-management#minio-authentication-and-identity-management|authentication]].

EXAMPLE

The following command applies the JSON-formatted anonymous policy
to the `mydata` bucket on the `myminio` MinIO deployment:

```
mc anonymous set-json ~/mydata-anonymous.json myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] set-json POLICY ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

POLICY[](#mc.anonymous.set-json.POLICY)

Required The path to the JSON-formatted policy to assign to the specified
`ALIAS`.

ALIAS[](#mc.anonymous.set-json.ALIAS)

Required The full path to the bucket or bucket prefix to which the
command applies the specified [`POLICY`](#mc.anonymous.set-json.POLICY).

Specify the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO or other
S3-compatible service and the full path to the bucket or bucket
prefix. For example:

```
mc anonymous set-json public play/mybucket
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Set Anonymous Policy for Bucket

Use [`mc anonymous set-json`](#command-mc.anonymous.set-json) to set the anonymous policy for a
bucket:

```
mc anonymous set-json POLICY ALIAS/PATH
```

Replace [`POLICY`](#mc.anonymous.set-json.POLICY) with a supported
[`POLICY`](#mc.anonymous.set-json.POLICY).

Replace [`ALIAS`](#mc.anonymous.set-json.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`PATH`](#mc.anonymous.set-json.ALIAS) with the destination bucket.

### Remove Anonymous Policy for Bucket

Use [[reference/minio-mc/mc-anonymous-set#command-mc.anonymous.set|`mc anonymous set`]] to clear the anonymous policy for a
bucket:

```
mc anonymous set none ALIAS/PATH
```

Replace [[reference/minio-mc/mc-anonymous-set#mc.anonymous.set.ALIAS|`ALIAS`]] with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [[reference/minio-mc/mc-anonymous-set#mc.anonymous.set.ALIAS|`PATH`]] with the destination bucket.

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