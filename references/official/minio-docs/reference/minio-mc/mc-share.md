---
title: "mc share — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-share.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-share"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc share — SILO (MinIO Object Storage)"
---
# mc share

## Description

Use the [`mc share`](#command-mc.share) commands to manage presigned URLs for downloading and uploading objects to a MinIO bucket.

## Subcommands

[`mc share`](#command-mc.share) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-share-download#command-mc.share.download|`download`]]

The [[reference/minio-mc/mc-share-download#command-mc.share.download|`mc share download`]] command generates a temporary presigned URL with
integrated access credentials for downloading objects from a MinIO bucket. The
temporary URL expires after a configurable time limit.

[[reference/minio-mc/mc-share-list#command-mc.share.list|`list`]]

The [[reference/minio-mc/mc-share-list#command-mc.share.ls|`mc share ls`]] command displays any unexpired presigned URLs generated
by [[reference/minio-mc/mc-share-upload#command-mc.share.upload|`mc share upload`]] or [[reference/minio-mc/mc-share-download#command-mc.share.download|`mc share download`]]

[[reference/minio-mc/mc-share-upload#command-mc.share.upload|`upload`]]

The [[reference/minio-mc/mc-share-upload#command-mc.share.upload|`mc share upload`]] command generates a temporary presigned URL with
integrated access credentials for uploading objects to a MinIO bucket. The
temporary URL expires after a configurable time limit.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).