---
title: "mc alias — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-alias.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-alias"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc alias — SILO (MinIO Object Storage)"
---
# mc alias

## Description

The [`mc alias`](#command-mc.alias) commands provide a convenient interface for managing the list of S3-compatible hosts that [[reference/minio-mc#command-mc|`mc`]] can connect to and run operations against.

Important

[[reference/minio-mc#command-mc|`mc`]] commands that operate on S3-compatible services require specifying an alias for that service.

## Subcommands

[`mc alias`](#command-mc.alias) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-alias-list#command-mc.alias.list|`list`]]

The [[reference/minio-mc/mc-alias-list#command-mc.alias.list|`mc alias list`]] command lists all aliases in the local
mc configuration.

[[reference/minio-mc/mc-alias-remove#command-mc.alias.remove|`remove`]]

The [[reference/minio-mc/mc-alias-remove#command-mc.alias.remove|`mc alias remove`]] removes an existing alias from the local
mc configuration.

[[reference/minio-mc/mc-alias-set#command-mc.alias.set|`set`]]

The [[reference/minio-mc/mc-alias-set#command-mc.alias.set|`mc alias set`]] command adds or updates an alias to the local
mc configuration.

[[reference/minio-mc/mc-alias-import#command-mc.alias.import|`import`]]

The [[reference/minio-mc/mc-alias-import#command-mc.alias.import|`mc alias import`]] command imports an alias configuration from a JSON document.

[[reference/minio-mc/mc-alias-export#command-mc.alias.export|`export`]]

The [[reference/minio-mc/mc-alias-export#command-mc.alias.export|`mc alias export`]] command exports an alias configuration from the existing [[reference/minio-mc#mc-configuration|configuration]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).