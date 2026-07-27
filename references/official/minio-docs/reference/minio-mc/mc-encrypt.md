---
title: "mc encrypt — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-encrypt.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-encrypt"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc encrypt — SILO (MinIO Object Storage)"
---
# mc encrypt

## Description

The [`mc encrypt`](#command-mc.encrypt) commands set, update, or disable the default bucket Server-Side Encryption (SSE) mode.
MinIO automatically encrypts objects using the specified SSE mode.

## Subcommands

[`mc encrypt`](#command-mc.encrypt) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-encrypt-clear#command-mc.encrypt.clear|`clear`]]

The [[reference/minio-mc/mc-encrypt-clear#command-mc.encrypt.clear|`mc encrypt clear`]] command removes the current default
encryption settings for a bucket.

[[reference/minio-mc/mc-encrypt-info#command-mc.encrypt.info|`info`]]

The [[reference/minio-mc/mc-encrypt-info#command-mc.encrypt.info|`mc encrypt info`]] command returns the current default
encryption settings for a bucket.

[[reference/minio-mc/mc-encrypt-set#command-mc.encrypt.set|`set`]]

The [[reference/minio-mc/mc-encrypt-set#command-mc.encrypt.set|`mc encrypt set`]] encrypt command sets or updates the default
bucket [[administration/server-side-encryption#minio-sse|Server-Side Encryption (SSE) mode]]. MinIO automatically
encrypts objects written to that bucket using the specified SSE mode.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).