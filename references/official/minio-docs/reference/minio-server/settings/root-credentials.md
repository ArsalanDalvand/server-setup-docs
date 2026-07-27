---
title: "Root Access Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/root-credentials.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/root-credentials"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Root Access Settings — SILO (MinIO Object Storage)"
---
# Root Access Settings

This page covers settings that control root (superuser) access for the MinIO process.
The root user has complete access and permissions to perform operations on the MinIO deployment.

Root User and Root Password are required even if you use the [MinIO Key Encryption Service](https://docs.min.io/community/minio-kes/) or other key management utility.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Root User

Environment Variable

MINIO_ROOT_USER[](#envvar.MINIO_ROOT_USER)

The access key for the [[administration/identity-access-management/minio-user-management#minio-users-root|root]] user.

Warning

If [`MINIO_ROOT_USER`](#envvar.MINIO_ROOT_USER) is unset, [[reference/minio-server/minio-server#command-minio|`minio`]] defaults to `minioadmin`.

NEVER use the default credentials in production environments.
MinIO strongly recommends specifying a unique, long, and random [`MINIO_ROOT_USER`](#envvar.MINIO_ROOT_USER) value for all environments.

Configuration Setting

This setting does not have a configuration variable setting.
Use the Environment Variable instead.

## Root Password

Environment Variable

MINIO_ROOT_PASSWORD[](#envvar.MINIO_ROOT_PASSWORD)

The secret key for the [[administration/identity-access-management/minio-user-management#minio-users-root|root]] user.

Warning

If [`MINIO_ROOT_PASSWORD`](#envvar.MINIO_ROOT_PASSWORD) is unset, [[reference/minio-server/minio-server#command-minio|`minio`]] defaults to `minioadmin`.

NEVER use the default credentials in production environments.
MinIO strongly recommends specifying a unique, long, and random [`MINIO_ROOT_PASSWORD`](#envvar.MINIO_ROOT_PASSWORD) value for all environments.

Configuration Setting

This setting does not have a configuration variable setting.
Use the Environment Variable instead.

## Root Access

Environment Variable

MINIO_API_ROOT_ACCESS[](#envvar.MINIO_API_ROOT_ACCESS)

Configuration Setting

api root-access[](#mc-conf.api.root-access)

New in version MinIO: Server RELEASE.2023-05-04T21-44-30Z

Specify `on` to enable and `off` to disable the [[administration/identity-access-management/minio-user-management#minio-users-root|root]] user account.
Disabling the root service account also disables all service accounts associated with root, excluding those used by site replication.
Defaults to `on`.

Important

If you disable root API access with this setting, you must still set a root user and a root password for internal use.

Ensure you have at least one other admin user, such as one with the [[administration/identity-access-management/policy-based-access-control#userpolicy.consoleAdmin|`consoleAdmin`]] policy, before disabling the root account.
If you do not have another admin user, disabling the root account locks administrative access to the deployment.

You can use this variable to temporarily override the configuration setting and re-enable root access to the deployment.

To reset after an unintentional lock, set [`MINIO_API_ROOT_ACCESS`](#envvar.MINIO_API_ROOT_ACCESS) `on` to override this setting and temporarily re-enable the root account.
You can then change this setting to `on` or make the necessary user/policy changes to ensure normal administrative access through other non-root accounts.

## Unique Root Credentials

New in version Server: RELEASE.2024-03-03T17-50-39Z

MinIO automatically generates unique root credentials if all of the following conditions are true:

[KES](https://docs.min.io/community/minio-kes/tutorials/getting-started/) Release 2024-03-01T18-06-46Z or later running

Have not defined:

`MINIO_ROOT_USER` variable

`MINIO_ROOT_PASSWORD` variable

Have:

set up KES with a [supported KMS target](https://docs.min.io/community/minio-kes/#supported-kms-targets)

disabled root access with the [MinIO environment variable](#minio-disable-root-access)

When those conditions are met at startup, MinIO uses the KMS to generate unique root credentials for the deployment using a [hash-based message authentication code (HMAC)](https://en.wikipedia.org/wiki/HMAC).

If MinIO generates such credentials, the key used to generate the credentials must remain the same and continue to exist.
All data on the deployment is encrypted with this key!

To rotate the generated root credentials, generate a new key in the KMS, then update the value of the [[reference/minio-server/settings/kes#envvar.MINIO_KMS_KES_KEY_NAME|`MINIO_KMS_KES_KEY_NAME`]] with the new key.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).