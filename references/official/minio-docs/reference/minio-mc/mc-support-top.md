---
title: "mc support top — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-support-top.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-support-top"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc support top — SILO (MinIO Object Storage)"
---
# mc support top

Note

Changed in version RELEASE.2022-08-11T00-30-48Z.

`mc support top` replaces the `mc admin top` command.

SUBNET Registration Required

The `mc support` commands are designed for MinIO deployments registered with [MinIO SUBNET](https://min.io/pricing?jmp=docs) to ensure optimal outcome of diagnostics and performance testing.
Deployments not registered with SUBNET cannot use the `mc support` commands.

## Description

The [`mc support top`](#command-mc.support.top) command returns statistics for distributed
MinIO deployments, similar to the output of the `top` command in a shell.

Note

[`mc support top`](#command-mc.support.top) is not supported on single-node single-drive MinIO deployments.

[`mc support top`](#command-mc.support.top) has the following subcommands:

[[reference/minio-mc/mc-support-top-api#command-mc.support.top.api|`api`]]

[[reference/minio-mc/mc-support-top-locks#command-mc.support.top.locks|`locks`]]

[[reference/minio-mc/mc-support-top-disk#command-mc.support.top.disk|`disk`]]

[[reference/minio-mc/mc-support-top-net#command-mc.support.top.net|`net`]]

[[reference/minio-mc/mc-support-top-rpc#command-mc.support.top.rpc|`rpc`]]

Refer to the pages linked above for each subcommand for details.

## Syntax

The command has the following syntax:

```
mc support top COMMAND [COMMAND FLAGS] [ARGUMENTS ...]
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).