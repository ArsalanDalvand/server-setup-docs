---
title: "mc retention — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-retention.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-retention"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc retention — SILO (MinIO Object Storage)"
---
# mc retention

## Description

The [`mc retention`](#command-mc.retention) command configures the [[administration/object-management/object-retention#minio-object-locking|Write-Once Read-Many (WORM) locking]] settings for an object or object(s) in a bucket.
You can also set the default object lock settings for a bucket, where all objects without explicit object lock settings inherit the bucket default.

## Subcommands

[`mc retention`](#command-mc.retention) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-retention-clear#command-mc.retention.clear|`clear`]]

The [[reference/minio-mc/mc-retention-clear#command-mc.retention.clear|`mc retention clear`]] command removes the
[[administration/object-management/object-retention#minio-object-locking|Write-Once Read-Many (WORM) locking]] settings for
an object or object(s) in a bucket. You can also remove the default object lock
settings for a bucket.

[[reference/minio-mc/mc-retention-info#command-mc.retention.info|`info`]]

The [[reference/minio-mc/mc-retention-info#command-mc.retention.info|`mc retention info`]] command configures the [[administration/object-management/object-retention#minio-object-locking|Write-Once Read-Many (WORM)
locking]] settings for an object or object(s) in a bucket.
You can also set the default object lock settings for a bucket, where all
objects without explicit object lock settings inherit the bucket default.

[[reference/minio-mc/mc-retention-set#command-mc.retention.set|`set`]]

The [[reference/minio-mc/mc-retention-set#command-mc.retention.set|`mc retention set`]] command configures the
[[administration/object-management/object-retention#minio-object-locking|Write-Once Read-Many (WORM) locking]] settings for
an object or object(s) in a bucket. You can also set the default object lock
settings for a bucket, where all objects without explicit object lock settings
inherit the bucket default.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).