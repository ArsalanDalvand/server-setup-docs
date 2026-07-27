---
title: "mc admin top — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-admin-top.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-admin-top"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin top — SILO (MinIO Object Storage)"
---
# mc admin top

Changed in version RELEASE.2022-08-11T00-30-48Z: `mc admin top` replaced by [[reference/minio-mc/mc-support-top#command-mc.support.top|`mc support top`]].

## Description

The [`mc admin top`](#command-mc.admin.top) command returns statistics for distributed
MinIO deployments, similar to the output of the `top` command.

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

## Syntax

mc admin top locks[](#mc.admin.top.locks)

Returns the 10 oldest locks on the MinIO deployment.

The command has the following syntax:

```
mc admin top locks TARGET
```

The command supports the following arguments:

TARGET[](#mc.admin.top.locks.TARGET)

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment from which
the command retrieves statistics.

The alias must correspond to a distributed (multi-node) MinIO deployment.
The command returns an error for [[glossary#term-single-node-single-drive|single-node single-drive]] deployments.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).