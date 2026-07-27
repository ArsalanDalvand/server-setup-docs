---
title: "mc admin speedtest — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-admin-speedtest.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-admin-speedtest"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin speedtest — SILO (MinIO Object Storage)"
---
# mc admin speedtest

Changed in version RELEASE.2022-07-24T02-25-13Z: `mc admin speedtest` replaced by [[reference/minio-mc/mc-support-perf#command-mc.support.perf|`mc support perf`]].

## Description

The [`mc admin speedtest`](#command-mc.admin.speedtest) command tests throughputs per host with `PUT`
and `GET` operations.

[`speedtest`](#command-mc.admin.speedtest) is available starting with `mc`
[RELEASE.2021-09-02T09-21-27Z](https://github.com/minio/mc/releases/tag/RELEASE.2021-09-02T09-21-27Z) and supports distributed MinIO
deployments running [RELEASE.2021-07-30T00-02-00Z](https://github.com/minio/minio/releases/tag/RELEASE.2021-07-30T00-02-00Z) or later.

[`speedtest`](#command-mc.admin.speedtest) does not support standalone or MinIO Gateway
deployments.

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

## Syntax

[`mc admin speedtest`](#command-mc.admin.speedtest) has the following syntax:

```
mc admin speedtest [FLAGS] TARGET
```

[`mc admin speedtest`](#command-mc.admin.speedtest) supports the following arguments:

TARGET[](#mc.admin.speedtest.TARGET)

Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment to run the speedtest against.

--duration[](#mc.admin.speedtest.-duration)

The duration the entire speedtests are run. Defaults to `10s`.

--size[](#mc.admin.speedtest.-size)

The size of the objects used for uploads/downloads. Defaults to `64MiB`.

--concurrent[](#mc.admin.speedtest.-concurrent)

The number of concurrent requests per server. Defaults to `32`.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).