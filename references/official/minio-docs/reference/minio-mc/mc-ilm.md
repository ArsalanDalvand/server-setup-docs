---
title: "mc ilm — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-ilm.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-ilm"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc ilm — SILO (MinIO Object Storage)"
---
# mc ilm

## Description

The [`mc ilm`](#command-mc.ilm) commands manage [[administration/object-management/object-lifecycle-management#minio-lifecycle-management|object lifecycle management rules]] and tiering on a MinIO deployment.

Use these command to

create tiers

create [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-tiering|tiering]] rules

manage [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|expiration]] rules for objects on a bucket

## Subcommands

[`mc ilm`](#command-mc.ilm) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-ilm-restore#command-mc.ilm.restore|`restore`]]

The [[reference/minio-mc/mc-ilm-restore#command-mc.ilm.restore|`mc ilm restore`]] command creates a temporary copy of an object archived
on a remote tier. The copy automatically expires after 1 day by default.

[[reference/minio-mc/mc-ilm-rule#command-mc.ilm.rule|`rule`]]

The [[reference/minio-mc/mc-ilm-rule#command-mc.ilm.rule|`mc ilm rule`]] command and its subcommands configure the rules used to transition objects between storage tiers in MinIO’s Lifecycle Management.

[[reference/minio-mc/mc-ilm-tier#command-mc.ilm.tier|`tier`]]

The [[reference/minio-mc/mc-ilm-tier#command-mc.ilm.tier|`mc ilm tier`]] command and its subcommands configure a remote supported S3-compatible service for MinIO [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|Lifecycle Management: Object Transition (“Tiering”)]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).