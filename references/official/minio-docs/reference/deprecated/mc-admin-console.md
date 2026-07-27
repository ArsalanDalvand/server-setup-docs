---
title: "mc admin console — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-admin-console.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-admin-console"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin console — SILO (MinIO Object Storage)"
---
# mc admin console

Important

This command has been replaced by [[reference/minio-mc-admin/mc-admin-logs#command-mc.admin.logs|`mc admin logs`]] in [mc RELEASE.2022-12-02T23-48-47Z](https://github.com/minio/mc/releases/tag/RELEASE.2022-12-02T23-48-47Z).

The command was previously replaced by `mc support logs show` in [mc RELEASE.2022-06-26T18-51-48Z](https://github.com/minio/mc/tree/RELEASE.2022-06-26T18-51-48Z).

## Description

The [`mc admin console`](#command-mc.admin.console) command returns server log entries for each
MinIO server in the deployment.

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

## Syntax

[`mc admin console`](#command-mc.admin.console) has the following syntax:

```
mc admin console [FLAGS] TARGET NODENAME
```

[`mc admin console`](#command-mc.admin.console) supports the following:

TARGET[](#mc.admin.console.TARGET)

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment from which
the command retrieves server logs.

NODENAME[](#mc.admin.console.NODENAME)

The specific MinIO server node from which the command retrieves server logs.

--limit, l[](#mc.admin.console.-limit)

The number of most recent log entries to show. Defaults to `10`.

--type, t[](#mc.admin.console.-type)

The type of errog logs to return. Specify one or more of the following
options as a comma-seperated `,` list:

`minio`

`application`

`all` (Default)

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).