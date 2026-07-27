---
title: "mc quota — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-quota.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-quota"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc quota — SILO (MinIO Object Storage)"
---
# mc quota

Changed in version RELEASE.2024-07-31T15-58-33Z: `mc quota` and its subcommands are deprecated.

## Description

The [`mc quota`](#command-mc.quota) commands configure, display, or remove a quota limit on a bucket.

When a bucket with a quota configured reaches the specified limit, as determined by the MinIO object scanner, MinIO rejects further `PUT` requests for the bucket.

Each time the MinIO [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-scanner|object scanner]] scans a bucket for pending [[administration/object-management/object-lifecycle-management#minio-lifecycle-management|object lifecycle transitions]], it also checks if the bucket has exceeded a configured quota.

Quota enforcement is not immediate

Bucket quotas are not intended to enforce a strict hard limit on a bucket’s size.
If a bucket exceeds its quota between scanner passes, MinIO continues to accept `PUT` requests for that bucket until _after_ the next scanner pass identifies the quota violation.

## Subcommands

[`mc quota`](#command-mc.quota) includes the following subcommands:

Subcommand

Description

[[reference/deprecated/mc-quota-clear#command-mc.quota.clear|`clear`]]

The [[reference/deprecated/mc-quota-clear#command-mc.quota.clear|`mc quota clear`]] command removes a configured storage quota for a bucket.

[[reference/deprecated/mc-quota-info#command-mc.quota.info|`info`]]

The [[reference/deprecated/mc-quota-info#command-mc.quota.info|`mc quota info`]] command displays the currently configured quota for a bucket.

[[reference/deprecated/mc-quota-set#command-mc.quota.set|`set`]]

The [[reference/deprecated/mc-quota-set#command-mc.quota.set|`mc quota set`]] assigns a hard quota limit to a bucket beyond which MinIO does not allow writes.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).