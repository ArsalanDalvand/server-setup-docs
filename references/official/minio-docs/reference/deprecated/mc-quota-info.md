---
title: "mc quota info — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-quota-info.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-quota-info"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc quota info — SILO (MinIO Object Storage)"
---
# mc quota info

Changed in version RELEASE.2022-12-13T00-23-28Z: `mc quota info` replaced `mc admin bucket quota`.

Changed in version RELEASE.2024-07-31T15-58-33Z: `mc quota info` is deprecated.

## Description

The [`mc quota info`](#command-mc.quota.info) command displays the currently configured quota for a bucket.

## Examples

### Retrieve Bucket Quota Configuration

Use [`mc quota info`](#command-mc.quota.info) to retrieve the current quota configuration for a bucket:

```
mc quota info TARGET/BUCKET
```

Replace `TARGET` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.
Replace `BUCKET` with the name of the bucket on which to retrieve the quota.

## Syntax

[`mc quota info`](#command-mc.quota.info) has the following syntax:

```
mc quota info TARGET
```

[`mc quota info`](#command-mc.quota.info) supports the following arguments:

TARGET[](#mc.quota.info.TARGET)
Required

The full path to the bucket for which the command creates the quota.
Specify the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment as a prefix to the path.
For example:

```
mc quota play/mybucket
```

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

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