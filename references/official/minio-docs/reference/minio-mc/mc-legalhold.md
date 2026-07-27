---
title: "mc legalhold — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-legalhold.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-legalhold"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc legalhold — SILO (MinIO Object Storage)"
---
# mc legalhold

## Description

The [`mc legalhold`](#command-mc.legalhold) command sets, removes, or retrieves the [[administration/object-management/object-retention#minio-object-locking-legalhold|object legal hold (WORM)]] settings for object(s).

## Subcommands

[`mc legalhold`](#command-mc.legalhold) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-legalhold-clear#command-mc.legalhold.clear|`clear`]]

The [[reference/minio-mc/mc-legalhold-clear#command-mc.legalhold.clear|`mc legalhold clear`]] command removes the current [[administration/object-management/object-retention#minio-object-locking-legalhold|legal hold]] setting for an object or objects.

[[reference/minio-mc/mc-legalhold-info#command-mc.legalhold.info|`info`]]

The [[reference/minio-mc/mc-legalhold-info#command-mc.legalhold.info|`mc legalhold info`]] command returns the current [[administration/object-management/object-retention#minio-object-locking-legalhold|legal hold]] setting for an object or objects.

[[reference/minio-mc/mc-legalhold-set#command-mc.legalhold.set|`set`]]

The [[reference/minio-mc/mc-legalhold-set#command-mc.legalhold.set|`mc legalhold set`]] command enables [[administration/object-management/object-retention#minio-object-locking-legalhold|legal hold]] Write-Once Read-Many (WORM) object locking on
an object or objects.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).