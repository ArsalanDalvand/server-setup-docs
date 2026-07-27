---
title: "mc get — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-get.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-get"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc get — SILO (MinIO Object Storage)"
---
# mc get

New in version mc: RELEASE.2024-02-24T01-33-20Z

## Syntax

The [`mc get`](#command-mc.get) command downloads an object from a target S3 deployment to the local file system.

`mc get` provides a simplified interface for downloading files compared to [[reference/minio-mc/mc-cp#command-mc.cp|`mc cp`]] or [[reference/minio-mc/mc-mirror#command-mc.mirror|`mc mirror`]].
`mc get` uses a one-way download function that trades efficiency for the power and complexity of the other commands.

EXAMPLE

The following downloads the file `logo.png` from an s3 source to the local file system at path `~/images/collateral/`.

```
mc get minio/marketing/logo.png ~/images/collateral
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] get                      \
                 SOURCE                   \
                 TARGET                   \
                 [--enc-c string]         \
                 [--version-id, --vid value]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

SOURCE[](#mc.get.SOURCE)
Required

The full path to the [[reference/minio-mc/mc-alias-set#minio-mc-alias|alias]], bucket, prefix (if used), and object to download.

TARGET[](#mc.get.TARGET)
Required

The destination path on the local file system where the command should place the downloaded file.

--enc-c[](#mc.get.-enc-c)
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

--version-id, --vid[](#mc.get.-version-id)
Optional

Retrieve a specific version of the object.
Pass the version ID of the object to retrieve.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Retrieve an object from MinIO to the local file system

The following command retrieves the file `myobject.csv` from the bucket `mybucket` at the alias `myminio` and places it on the local file system at the path `/my/local/folder`.

```
mc get myminio/mybucket/myobject.csv /my/local/folder
```

### Retrieve an encrypted object from MinIO

The following command retrieves an encrypted file and places it at a local folder path.

```
mc get --enc-c "play/mybucket/object=MDEyMzQ1Njc4OTAxMjM0NTY3ODkwMTIzNDU2Nzg5MDA" play/mybucket/object path-to/object
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).