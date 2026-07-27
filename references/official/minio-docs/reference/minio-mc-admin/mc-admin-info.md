---
title: "mc admin info — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-info.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-info"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin info — SILO (MinIO Object Storage)"
---
# mc admin info

## Description

The [`mc admin info`](#command-mc.admin.info) command displays information on a MinIO server.
For distributed MinIO deployments, [`mc admin info`](#command-mc.admin.info) displays information
for each MinIO server in the deployment.

New in version mc: RELEASE.2024-05-03T11-21-07Z

The command output includes information about the [[operations/concepts/erasure-coding#minio-ec-erasure-set|erasure code]] setting for the cluster.
This displays in the output in the format `EC:#`.

The output of the command resembles the following:

```
●  play.min.io
   Uptime: 2 hours
   Version: 2024-05-10T08:24:14Z
   Network: 1/1 OK
   Drives: 4/4 OK
   Pool: 1

Pools:
   1st, Erasure sets: 1, Drives per erasure set: 4

0 B Used, 3 Buckets, 0 Objects
4 drives online, 0 drives offline, EC:1
```

## Examples

The following example assumes that the `play` alias exists in the
[[reference/minio-mc#command-mc|`mc`]] [[reference/minio-mc#mc-configuration|configuration file]]. You can
replace `play` with the alias for your preferred S3-compatible deployment.

See [[reference/minio-mc/mc-alias#command-mc.alias|`mc alias`]] for more information on aliases.

```
mc admin info play
```

## Syntax

[`mc admin info`](#command-mc.admin.info) has the following syntax:

```
mc admin info TARGET      \
              [--offline]
```

Specify the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment as the `TARGET`.

### Parameters

TARGET[](#mc.admin.info.TARGET)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] about which you want to display information.

--offline[](#mc.admin.info.-offline)
Optional

Show only offline drives or nodes.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).