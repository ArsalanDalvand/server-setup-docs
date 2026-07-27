---
title: "mc admin bucket quota — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-admin-bucket-quota.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-admin-bucket-quota"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin bucket quota — SILO (MinIO Object Storage)"
---
# mc admin bucket quota

Changed in version RELEASE.2022-12-13T00-23-28Z: `mc admin bucket quota` replaced by:

[[reference/deprecated/mc-quota-set#command-mc.quota.set|`mc quota set`]]

[[reference/deprecated/mc-quota-info#command-mc.quota.info|`mc quota info`]]

[[reference/deprecated/mc-quota-clear#command-mc.quota.clear|`mc quota clear`]]

## Description

The [`mc admin bucket quota`](#command-mc.admin.bucket.quota) command manages per-bucket
storage quotas.

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

### Units of Measurement

The [`mc admin bucket quota --hard`](#mc.admin.bucket.quota.-hard) flag
accepts the following case-insensitive suffixes to represent the unit of the
specified size value:

Suffix

Unit Size

`k`

KB (Kilobyte, 1000 Bytes)

`m`

MB (Megabyte, 1000 Kilobytes)

`g`

GB (Gigabyte, 1000 Megabytes)

`t`

TB (Terrabyte, 1000 Gigabytes)

`ki`

KiB (Kibibyte, 1024 Bites)

`mi`

MiB (Mebibyte, 1024 Kibibytes)

`gi`

GiB (Gibibyte, 1024 Mebibytes)

`ti`

TiB (Tebibyte, 1024 Gibibytes)

Omitting the suffix defaults to `bytes`.

## Examples

### Configure a Hard Quota on a Bucket

Use [`mc admin bucket quota`](#command-mc.admin.bucket.quota) with the
[`--hard`](#mc.admin.bucket.quota.-hard) flag to specify a hard quota
on a bucket. Hard quotas prevent the bucket size from growing past the specified
limit.

```
mc admin bucket quota TARGET/BUCKET --hard LIMIT
```

Replace `TARGET` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured
MinIO deployment. Replace `BUCKET` with the name of the bucket on which to
set the hard quota.

Replace `LIMIT` with the maximum size to which the bucket can grow.
For example, to set a hard limit of 10 Terrabytes, specify `10t`.
See [Units of Measurement](#mc-admin-bucket-quota-units) for supported units.

### Retrieve Bucket Quota Configuration

Use [`mc admin bucket quota`](#command-mc.admin.bucket.quota) to retrieve the current quota configuration
for a bucket:

```
mc admin bucket quota TARGET/BUCKET
```

Replace `TARGET` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured
MinIO deployment. Replace `BUCKET` with the name of the bucket on which to
retrieve the quota.

### Clear Configured Bucket Quota

Use [`mc admin bucket quota`](#command-mc.admin.bucket.quota) with the
[`--clear`](#mc.admin.bucket.quota.-clear) flag to clear all quotas from
a bucket.

```
mc admin bucket quota TARGET/BUCKET --clear
```

Replace `TARGET` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured
MinIO deployment. Replace `BUCKET` with the name of the bucket on which to
clear the quota.

## Syntax

[`mc admin bucket quota`](#command-mc.admin.bucket.quota) has the following syntax:

```
mc admin bucket quota TARGET [ARGUMENTS]
```

[`mc admin bucket quota`](#command-mc.admin.bucket.quota) supports the following arguments:

TARGET[](#mc.admin.bucket.quota.TARGET)

The full path to the bucket for which the command creates the quota.
Specify the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment as a
prefix to the path. For example:

```
mc admin bucket quota play/mybucket
```

Omit all other arguments to return the current quota settings for the
specified bucket.

--hard[](#mc.admin.bucket.quota.-hard)

Sets a maximum limit to the bucket storage size. The MinIO server rejects any
incoming `PUT` request whose contents would exceed the bucket’s configured
quota.

For example, a hard limit of `10GB` would prevent adding any additional
objects if the bucket reaches `10GB` of size.

See [Units of Measurement](#mc-admin-bucket-quota-units) for supported unit sizes.

--clear[](#mc.admin.bucket.quota.-clear)

Clears all quotas configured for the bucket.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).