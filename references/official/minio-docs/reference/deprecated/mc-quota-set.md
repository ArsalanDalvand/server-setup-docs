---
title: "mc quota set — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-quota-set.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-quota-set"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc quota set — SILO (MinIO Object Storage)"
---
# mc quota set

Changed in version RELEASE.2022-12-13T00-23-28Z: `mc quota set` replaced `mc admin bucket quota --hard`.

Changed in version RELEASE.2024-07-31T15-58-33Z: `mc quota set` is deprecated.

## Description

The [`mc quota set`](#command-mc.quota.set) assigns a hard quota limit to a bucket beyond which MinIO does not allow writes.

### Units of Measurement

The [`mc quota set --size`](#mc.quota.set.-size) flag accepts the following case-insensitive suffixes to represent the unit of the specified size value:

Suffix

Unit Size

`k`

KB (Kilobyte, 1000 Bytes)

`m`

MB (Megabyte, 1000 Kilobytes)

`g`

GB (Gigabyte, 1000 Megabytes)

`t`

TB (Terabyte, 1000 Gigabytes)

`ki` or `kib`

KiB (Kibibyte, 1024 Bites)

`mi` or `mib`

MiB (Mebibyte, 1024 Kibibytes)

`gi` or `gib`

GiB (Gibibyte, 1024 Mebibytes)

`ti` or `tib`

TiB (Tebibyte, 1024 Gibibytes)

Omitting a suffix defaults to `bytes`.

## Examples

### Configure a Hard Quota on a Bucket

Use [`mc quota set`](#command-mc.quota.set) with the [`--size`](#mc.quota.set.-size) flag to specify a hard quota on a bucket.
Hard quotas prevent the bucket size from growing past the specified limit.

```
mc quota set TARGET/BUCKET --size LIMIT
```

Replace `TARGET` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.
Replace `BUCKET` with the name of the bucket on which to set the hard quota.

Replace `LIMIT` with the maximum size to which the bucket can grow as an integer and, as desired, a suffix.
For example, to set a hard limit of 10 Terabytes, specify `10t`.

## Syntax

[`mc quota set`](#command-mc.quota.set) has the following syntax:

```
mc quota set TARGET --size LIMIT
```

[`mc quota set`](#command-mc.quota.set) supports the following arguments:

TARGET[](#mc.quota.set.TARGET)
Required

The full path to the bucket for which the command creates the quota.
Specify the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment as a prefix to the path.
For example:

```
mc quota set play/mybucket --size 10Gi
```

--size[](#mc.quota.set.-size)
Required

Sets a maximum limit to the bucket storage size.
The MinIO server rejects any incoming `PUT` request whose contents would exceed the bucket’s configured quota.

For example, a hard limit of `10G` would prevent adding any additional objects if the bucket reaches 10 gigabytes of size.

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