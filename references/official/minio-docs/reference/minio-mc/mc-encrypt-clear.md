---
title: "mc encrypt clear — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-encrypt-clear.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-encrypt-clear"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc encrypt clear — SILO (MinIO Object Storage)"
---
# mc encrypt clear

## Syntax

The [`mc encrypt clear`](#command-mc.encrypt.clear) command removes the current default
encryption settings for a bucket.

EXAMPLE

The following command removes the default encryption settings for
the `mydata` bucket on the MinIO deployment associated with the
`myminio` [[reference/minio-mc/mc-alias-set#alias|alias]]:

```
mc encrypt clear myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] encrypt clear ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.encrypt.clear.ALIAS)

The full path to the bucket on which to remove the default SSE mode.
Specify the [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment as the prefix to the
ALIAS path. For example:

```
mc encrypt clear play/mybucket
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Remove the Automatic Server-Side Encryption Settings for a Bucket

Example

```
mc encrypt clear myminio/data
```

Syntax

```
mc encrypt clear ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the
MinIO deployment on which to remove automatic server-side bucket
encryption.

## Behavior

### Modifying Bucket Encryption Settings Does Not Affect Encrypted Objects

Disabling automatic bucket encryption does not decrypt any objects in the
bucket.

To permanently decrypt objects in the bucket, you can perform an in-place
copy after disabling object decryption. For versioned buckets, the
previous object versions remain encrypted.

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