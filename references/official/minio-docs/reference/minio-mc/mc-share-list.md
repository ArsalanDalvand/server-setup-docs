---
title: "mc share ls — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-share-list.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-share-list"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc share ls — SILO (MinIO Object Storage)"
---
# mc share ls

## Syntax

The [`mc share ls`](#command-mc.share.ls) command displays any unexpired presigned URLs generated
by [[reference/minio-mc/mc-share-upload#command-mc.share.upload|`mc share upload`]] or [[reference/minio-mc/mc-share-download#command-mc.share.download|`mc share download`]]

The [`mc share list`](#command-mc.share.list) command has equivalent functionality to [`mc share ls`](#command-mc.share.ls).

Applications can perform a `PUT` to retrieve the object from the URL.

For more information on shareable object URLs, see the Amazon S3
documentation on [Pre-Signed URLs](https://docs.aws.amazon.com/AmazonS3/latest/dev/ShareObjectPreSignedURL.html).

EXAMPLE

The following command lists all upload and download presigned URLs
respectively for the `mydata` bucket on the `myminio` MinIO
deployment:

```
mc share ls upload myminio/mydata
mc share ls download myminio/mydata
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] share list           \
                 [download | upload]  \
                 ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

download[](#mc.share.ls.download)

Required Lists all unexpired presigned download (`GET`) URLs.

Mutually exclusive with [`mc share ls upload`](#mc.share.ls.upload)

upload[](#mc.share.ls.upload)

Required Lists all unexpired presigned upload (`PUT`) URLs.

Mutually exclusive with [`mc share ls download`](#mc.share.ls.download)

ALIAS[](#mc.share.ls.ALIAS)

Required The [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deplyment and the full path to
the object for which to list unexpired presigned URLs.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### List Generated Download and Upload URLs

List Active Download Presigned URLs

Use [`mc share ls download`](#mc.share.ls.download) to generate a URL
that supports `POST` requests for uploading a file to a specific object
location on an S3-compatible host:

```
mc share ls download ALIAS
```

Replace [`ALIAS`](#mc.share.ls.ALIAS) with the
[[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

List Active Upload Presigned URLs

Use [`mc share ls upload`](#mc.share.ls.upload) to generate a URL that
supports `POST` requests for uploading a file to a specific object
location on an S3-compatible host:

```
mc share ls upload ALIAS
```

Replace [[reference/minio-mc/mc-share-upload#mc.share.upload.ALIAS|`ALIAS`]] with the
[[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment.

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