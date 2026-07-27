---
title: "mc put — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-put.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-put"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc put — SILO (MinIO Object Storage)"
---
# mc put

New in version mc: RELEASE.2024-02-24T01-33-20Z

## Syntax

The [`mc put`](#command-mc.put) uploads an object from the local file system to a bucket on a target S3 deployment.

`mc put` provides a simplified interface for uploading files compared to [[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] or [[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]].
`mc put` uses a one-way upload function that trades efficiency for the power and complexity of the other commands.

EXAMPLE

The following uploads the file `logo.png` from the local file system at path `~/images/collateral/` to a bucket called `marketing` on the MinIO deployment with the alias of `minio`.

```
mc put ~/images/collateral/logo.png minio/marketing
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] put                            \
                 TARGET                         \
                 [--checksum value]             \
                 [--disable-multipart]          \
                 [--enc-kms value]              \
                 [--enc-s3 value]               \
                 [--enc-c value]                \
                 [--if-not-exists]              \
                 [--parallel, -P integer]       \
                 [--part-size, -s string]       \
                 [--storage-class, -sc string]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

TARGET[](#mc.put.TARGET)
Required

The full path to the [[reference/minio-mc/mc-alias-set#minio-mc-alias|alias]] or prefix where the command should run.
The TARGET must contain an [[reference/minio-mc/mc-alias-set#alias|alias]] and `bucket` name.

The TARGET may also contain the following optional components:
- PREFIX where the object should upload to
- OBJECT-NAME to use in place of the file names

Valid TARGETs could take any of the following forms:
- `ALIAS/BUCKET`
- `ALIAS/BUCKET/PREFIX`
- `ALIAS/BUCKET/OBJECT-NAME`
- `ALIAS/BUCKET/PREFIX/OBJECT-NAME`

--checksum[](#mc.put.-checksum)
Optional

New in version RELEASE.2024-10-02T08-27-28Z.

Add a checksum to an uploaded object.

Valid values are:
- `MD5`
- `CRC32`
- `CRC32C`
- `SHA1`
- `SHA256`

The flag requires server trailing headers and works with AWS or MinIO targets.

--disable-multipart[](#mc.put.-disable-multipart)
Optional

New in version RELEASE.2024-10-02T08-27-28Z.

Disables multipart uploads and directs `mc` to send the object in a single `PUT` operation.

--enc-kms[](#mc.put.-enc-kms)

Encrypt or decrypt objects using server-side [[administration/server-side-encryption#minio-sse|SSE-KMS encryption]] with client-managed keys.

The parameter accepts a key-value pair formatted as `KEY=VALUE`

`KEY`

The full path to the object as `alias/bucket/path/object.ext`.

You can specify only the top-level path to use a single encryption key for all operations in that path.

`VALUE`

Specify an existing data key on the external KMS.

See the [[reference/minio-mc-admin/mc-admin-kms-key#mc.admin.kms.key.create|`mc admin kms key create`]] reference for creating data keys.

For example:

```
--enc-kms "myminio/mybucket/prefix/object.obj=mybucketencryptionkey"
```

You can specify multiple encryption keys by repeating the parameter.

Specify the path to a prefix to apply encryption to all matching objects at that path:

```
--enc-kms "myminio/mybucket/prefix/=mybucketencryptionkey"
```

--enc-s3[](#mc.put.-enc-s3)
Optional

Encrypt or decrypt objects using server-side [[administration/server-side-encryption#minio-sse|SSE-S3 encryption]] with KMS-managed keys.
Specify the full path to the object as `alias/bucket/prefix/object`.

For example:

```
--enc-s3 "myminio/mybucket/prefix/object.obj"
```

You can specify the parameter multiple times to denote different object(s) to encrypt:

```
--enc-s3 "myminio/mybucket/foo/fooobject.obj" --enc-s3 "myminio/mybucket/bar/barobject.obj"
```

Specify the path to a prefix to apply encryption to all matching objects at that path:

```
--enc-s3 "myminio/mybucket/foo"
```

--enc-c[](#mc.put.-enc-c)
Optional

Encrypt or decrypt objects using server-side [[administration/server-side-encryption#minio-sse|SSE-C encryption]] with client-managed keys.

The parameter accepts a key-value pair formatted as `KEY=VALUE`

`KEY`

The full path to the object as `alias/bucket/path/object.ext`.

You can specify only the top-level path to use a single encryption key for all operations in that path.

`VALUE`

Specify either a 32-byte RawBase64-encoded key or a 64-byte hex-encoded key for use with SSE-C encryption.

Raw Base64 encoding rejects `=`-padded keys.
Omit the padding or use a Base64 encoder that supports RAW formatting.

`KEY` - the full path to the object as `alias/bucket/path/object`.

`VALUE` - the 32-byte RAW Base64-encoded data key to use for encrypting object(s).

For example:

```
# RawBase64-Encoded string "mybucket32byteencryptionkeyssec"
--enc-c "myminio/mybucket/prefix/object.obj=bXlidWNrZXQzMmJ5dGVlbmNyeXB0aW9ua2V5c3NlYwo"
```

You can specify multiple encryption keys by repeating the parameter.

Specify the path to a prefix to apply encryption to all matching objects at that path:

```
--enc-c "myminio/mybucket/prefix/=bXlidWNrZXQzMmJ5dGVlbmNyeXB0aW9ua2V5c3NlYwo"
```

Note

MinIO strongly recommends against using SSE-C encryption in production workloads.
Use SSE-KMS via the `--enc-kms` or SSE-S3 via `--enc-s3` parameters instead.

--parallel, --P[](#mc.put.-parallel)
Optional

For multi-part uploads, specify the number of parts of the object to upload in parallel.

If not defined, defaults to a value of `4`.

--part-size, -s[](#mc.put.-part-size)
Optional

Specify the size to use for each part of a multi-part upload.

If not defined, defaults to a value of `16MiB`.

--storage-class, -sc[](#mc.put.-storage-class)
Optional

Set the storage class for the uploaded object.

See [[reference/minio-server/settings/storage-class#minio-ec-storage-class-standard|Standard Storage Class]] for more about storage classes.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Upload a File and Specify the Object Name

The following command uploads the file `logo.png` from the local file system to the `business` bucket on the `minio` deployment, uploading it on the destination as `company-logo.png`.

```
mc put images/collateral/logo.png minio/business/company-logo.png
```

### Upload a Multipart Object in Parallel with a Specified Part Size

The following command uploads a file in chunks of 20MiB each and uploads 8 parts of the file in parallel.
8 parts are uploaded in succession until all parts of the object have uploaded.

```
mc put ~/videos/collateral/splash-page.mp4 minio/business --parallel 8 --part-size 20MiB
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).