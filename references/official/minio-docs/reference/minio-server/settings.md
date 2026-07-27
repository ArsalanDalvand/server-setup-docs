---
title: "Settings Overview — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Settings Overview — SILO (MinIO Object Storage)"
---
# Settings Overview

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process stores its configuration in the storage backend [[reference/minio-server/minio-server#minio.server.DIRECTORIES|`directory`]].

## MinIO Settings

MinIO settings define runtime behavior of the MinIO [[reference/minio-server/minio-server#command-minio.server|`server`]] process.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

Additional settings include those to customize:

[[reference/minio-server/settings/core#minio-server-envvar-core|Core settings]]

[[reference/minio-server/settings/root-credentials#minio-server-envvar-root|Root credentials]]

[[reference/minio-server/settings/storage-class#minio-server-envvar-storage-class|Storage class]]

[[reference/minio-server/settings/console#minio-server-envvar-console|MinIO Console]]

[[reference/minio-server/settings/metrics-and-logging#minio-server-envvar-metrics-logging|Metrics and logging]]

[[reference/minio-server/settings/notifications#minio-server-envvar-notifications|Notification targets]] for use with [[administration/monitoring/bucket-notifications#minio-bucket-notifications|MinIO Bucket Notifications]]

[[reference/minio-server/settings/iam#minio-server-envvar-iam|Identity and access management solutions]]

[[reference/minio-server/settings/kes#minio-server-envvar-kes|Key Encryption Service (KES)]]

[[reference/minio-server/settings/object-lambda#minio-server-envvar-object-lambda-webhook|Object Lambda functions]]

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).