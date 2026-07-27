---
title: "mc admin service — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-service.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-service"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin service — SILO (MinIO Object Storage)"
---
# mc admin service

## Description

The [`mc admin service`](#command-mc.admin.service) command can restart or unfreeze MinIO servers.

[`mc admin service`](#command-mc.admin.service) affects all MinIO servers in the target deployment
at the same time. The command interrupts in-progress API operations on
the MinIO deployment. Use caution when issuing this command to a deployment.

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

## Examples

### Restart MinIO Servers in Target Deployment

The following example uses the default `myminio` alias. The `myminio`
alias points to a local `minio` server running on port `9000`. See
<installation instructions> for more information on installing and running
a local `minio` server instance.

See [[reference/minio-mc/mc-alias#command-mc.alias|`mc alias`]] for more information on aliases.

```
mc admin service restart myminio
```

### Resume S3 Calls on a Target Deployment

The following example uses the default `myminio` alias. The `myminio`
alias points to a local `minio` server running on port `9000`. See
<installation instructions> for more information on installing and running
a local `minio` server instance.

See [[reference/minio-mc/mc-alias#command-mc.alias|`mc alias`]] for more information on aliases.

```
mc admin service unfreeze myminio
```

## Syntax

[`mc admin service`](#command-mc.admin.service) has the following syntax:

```
mc admin service COMMAND [ARGUMENTS]
```

[`mc admin service`](#command-mc.admin.service) supports the following commands:

restart[](#mc.admin.service.restart)

Restarts MinIO servers.
If needed, the command may suggest restarting the node based on the status.

[`mc admin service restart`](#mc.admin.service.restart) has the following syntax:

```
mc admin service restart ALIAS
```

Specify the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.
[`restart`](#mc.admin.service.restart) restarts all MinIO servers in the
deployment.

unfreeze[](#mc.admin.service.unfreeze)

Restart S3 API calls on a MinIO cluster.

[`mc admin service unfreeze`](#mc.admin.service.unfreeze) has the following syntax:

```
mc admin service unfreeze ALIAS
```

Specify the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).