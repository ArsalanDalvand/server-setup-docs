---
title: "mc anonymous set — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-anonymous-set.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-anonymous-set"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc anonymous set — SILO (MinIO Object Storage)"
---
# mc anonymous set

## Syntax

The [`mc anonymous set`](#command-mc.anonymous.set) command sets anonymous (i.e. unauthenticated or public)
access [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] for a bucket.

Buckets with anonymous policies allow clients to access the bucket contents
and perform actions consistent with the specified policy without
[[administration/identity-access-management#minio-authentication-and-identity-management|authentication]].

To set anonymous bucket policies using an IAM
[JSON policy](https://docs.aws.amazon.com/AmazonS3/latest/userguide/using-iam-policies), use the
[[reference/minio-mc/mc-anonymous-set-json#command-mc.anonymous.set-json|`mc anonymous set-json`]] command.

EXAMPLE

The following command sets anonymous access policies for several
buckets on the `myminio` MinIO deployment:

```
mc anonymous set upload myminio/uploads
mc anonymous set download myminio/downloads
mc anonymous set public myminio/public
```

Applications can perform the following operations without authentication:

`PUT` objects to `myminio/uploads` and `myminio/public`.

`GET` objects from `myminio/downloads` and `myminio/public`.

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] policy set PERMISSION ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

PERMISSION[](#mc.anonymous.set.PERMISSION)

Required Name of the policy to assign to the specified `ALIAS`.
Specify one of the following values:

`none` - Disable anonymous access to the `ALIAS`.

`download` - Enable download-only access to the `ALIAS`.

`upload` - Enable upload-only access to the `ALIAS`.

`public` - Enable download and upload access to the `ALIAS`.

ALIAS[](#mc.anonymous.set.ALIAS)

Required The full path to the bucket or bucket prefix to which the
command applies the specified [`PERMISSION`](#mc.anonymous.set.PERMISSION).

Specify the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO or other
S3-compatible service and the full path to the bucket or bucket
prefix. For example:

```
mc anonymous set public play/mybucket
```

Specify a bucket prefix to set the policy on only that prefix. For example,
this command sets distinct anonymous bucket policies on the
`mybucket/downloads` and `mybucket/uploads` prefixes:

```
mc anonymous set download play/mybucket/downloads
mc anonymous set upload play/mybucket/uploads
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Set Anonymous Policy for Bucket

Use [`mc anonymous set`](#command-mc.anonymous.set) to set the anonymous policy for a
bucket:

```
mc anonymous set POLICY ALIAS/PATH
```

Replace [`POLICY`](#mc.anonymous.set.PERMISSION) with a supported
[`permission`](#mc.anonymous.set.PERMISSION).

Replace [`ALIAS`](#mc.anonymous.set.ALIAS) with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured S3-compatible host.

Replace [`PATH`](#mc.anonymous.set.ALIAS) with the destination bucket.

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