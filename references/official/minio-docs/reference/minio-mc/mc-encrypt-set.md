---
title: "mc encrypt set — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-encrypt-set.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-encrypt-set"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc encrypt set — SILO (MinIO Object Storage)"
---
# mc encrypt set

## Syntax

The [`mc encrypt set`](#command-mc.encrypt.set) encrypt command sets or updates the default
bucket [[administration/server-side-encryption#minio-sse|Server-Side Encryption (SSE) mode]]. MinIO automatically
encrypts objects written to that bucket using the specified SSE mode.

[`mc encrypt set`](#command-mc.encrypt.set) only supports [[administration/server-side-encryption/server-side-encryption-sse-kms#minio-encryption-sse-kms|SSE-KMS]]
and [[administration/server-side-encryption/server-side-encryption-sse-s3#minio-encryption-sse-s3|SSE-S3]].

EXAMPLE

The following command sets the default
[[administration/server-side-encryption/server-side-encryption-sse-kms#minio-encryption-sse-kms|SSE-KMS encryption key]] for the bucket
`mydata` on the `myminio` MinIO deployment:

```
mc encrypt set sse-kms "minio-encryption-key" myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] encrypt set  ENCRYPTION [KMSKEY] ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ENCRYPTION[](#mc.encrypt.set.ENCRYPTION)

Specify the server-side encryption type to use as the default SSE mode.
Supports the following values:

`sse-kms` - Encrypt objects using the key specified in
[`KMSKEY`](#mc.encrypt.set.KMSKEY). MinIO
must have access to the specified key on the external KMS to
successfully encrypt or decrypt objects protected using SSE-KMS.

`sse-s3` - Encrypt objects using the key specified to
[[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_KEY_NAME|`MINIO_KMS_KES_KEY_NAME`]]. MinIO must have access to the
specified key on the external KMS to successfully encrypt or decrypt
objects protected using SSE-S3.

KMSKEY[](#mc.encrypt.set.KMSKEY)

Specify the KMS Master Key to use for performing SSE object encryption. This
option only applies if [`ENCRYPTION`](#mc.encrypt.set.ENCRYPTION) is
`sse-kms`.

Omit this option to direct MinIO to use the
[[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_KEY_NAME|`MINIO_KMS_KES_KEY_NAME`]].

ALIAS[](#mc.encrypt.set.ALIAS)

The full path to the bucket on which to set the default SSE mode. Specify the
[[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment as the prefix to the TARGET
path. For example:

```
mc encrypt set ENCRYPTION [KMSKEY] play/mybucket
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Enable Automatic Server-Side Bucket Encryption

Example

The following commands assumes that:

The MinIO server configuration supports
[[administration/server-side-encryption/server-side-encryption-sse-kms#minio-encryption-sse-kms|SSE-KMS]]

The root has an encryption key `minio-encryption-key`.

```
mc encrypt set sse-kms minio-encryption-key myminio/data
```

Syntax

```
mc encrypt set ENCRYPTION KMSKEY TARGET
```

Replace `ENCRYPTION` with `sse-kms` or `sse-s3` depending
on the preferred encryption mode.

Replace `KMSKEY` with the name of the encryption key on the
configured root KMS. This argument has no effect with `sse-s3`.

Replace `TARGET` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the
MinIO deployment on which to configure automatic server-side bucket
encryption.

## Behavior

[`mc encrypt set`](#command-mc.encrypt.set) makes no assumptions about the MinIO server’s current
encryption state. Specifying default encryption settings which the
server cannot support may result in undesired behavior.

Setting or modifying the default server-side encryption settings does not
automatically encrypt or decrypt the existing bucket contents.
If the bucket contents must have consistent encryption, use the
[[reference/minio-mc/mc-mv#command-mc.mv|`mc mv`]] command with [[reference/minio-mc/mc-mv#mc.mv.-enc-kms|`--enc-kms`]], [[reference/minio-mc/mc-mv#mc.mv.-enc-s3|`--enc-s3`]], or [[reference/minio-mc/mc-mv#mc.mv.-enc-c|`--enc-c`]] to specify the type of encryption to use for the moved contents.
This manually modifies the encryption settings or encrypted state of the bucket contents before changing the bucket default.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).