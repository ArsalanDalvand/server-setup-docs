---
title: "mc admin kms key — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-kms-key.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-kms-key"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin kms key — SILO (MinIO Object Storage)"
---
# mc admin kms key

## Description

The [`mc admin kms key`](#command-mc.admin.kms.key) command performs cryptographic key management
operations through the MinIO Key Encryption Service (KES).

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

## Syntax

mc admin kms key create[](#mc.admin.kms.key.create)

Creates a new master key on a Key Management System (KMS).

The command has the following syntax:

```
mc admin kms key create TARGET [KEY_NAME]
```

The command accepts the following arguments:

TARGET[](#mc.admin.kms.key.create.TARGET)

Specify the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

The `TARGET` deployment must include a configured
MinIO Key Encryption Service (KES) server.

KEY_NAME[](#mc.admin.kms.key.create.KEY_NAME)

Specify the name of the new master key.

mc admin kms key status[](#mc.admin.kms.key.status)

Requests information on a Key Management System (KMS) master key.

The command has the following syntax:

```
mc admin kms key status TARGET [KEY_NAME]
```

The command accepts the following arguments:

TARGET[](#mc.admin.kms.key.status.TARGET)

Specify the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

The `TARGET` deployment must include a configured MinIO Key
Encryption Service (KES) server.

KEY_NAME[](#mc.admin.kms.key.status.KEY_NAME)

Specify the name of a master key on the KMS.

Omit this argument to return the default master key on the
[`TARGET`](#mc.admin.kms.key.status.TARGET) deployment.

mc admin kms key list[](#mc.admin.kms.key.list)

List all Key Management System (KMS) keys for a MinIO instance.

The command has the following syntax:

```
mc admin kms key list TARGET
```

The command accepts the following argument:

TARGET[](#mc.admin.kms.key.list.TARGET)

Specify the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment.

The `TARGET` deployment must include a configured MinIO Key Encryption Service (KES) server.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).