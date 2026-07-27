---
title: "ILM Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/ilm.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/ilm"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "ILM Settings — SILO (MinIO Object Storage)"
---
# ILM Settings

This page covers settings that control Information Lifecycle Management (ILM) for the MinIO process.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Expiration Workers

Environment Variable

MINIO_ILM_EXPIRATION_WORKERS[](#envvar.MINIO_ILM_EXPIRATION_WORKERS)

Configuration Setting

ilm expiration_workers[](#mc-conf.ilm.expiration_workers)

New in version MinIO: Server RELEASE.2024-03-03T17-50-39Z

Set the number of workers to use for [[administration/object-management/object-lifecycle-management#minio-lifecycle-management-expiration|expiring objects]].
Valid values are `1` to `500`.

The default value is `100`.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).