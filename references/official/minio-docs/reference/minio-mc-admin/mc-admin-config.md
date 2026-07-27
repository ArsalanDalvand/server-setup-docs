---
title: "mc admin config — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-config.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-config"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin config — SILO (MinIO Object Storage)"
---
# mc admin config

## Description

The [`mc admin config`](#command-mc.admin.config) command manages configuration settings for the
[[reference/minio-server/minio-server#command-minio|`minio`]] server.

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

## Examples

## Syntax

mc admin config set[](#mc.admin.config.set)

Sets a [[reference/minio-server/settings#minio-server-configuration-settings|configuration key]] on the MinIO deployment.
Configurations defined by environment variables override configurations defined by this command.

mc admin config get[](#mc.admin.config.get)

Gets a [[reference/minio-server/settings#minio-server-configuration-settings|configuration key]] on the MinIO deployment created using mc admin config set.

mc admin config export[](#mc.admin.config.export)

Exports any configuration settings created using mc admin config set.

mc admin config history[](#mc.admin.config.history)

Lists the history of changes made to configuration keys by mc admin config.

Configurations defined by environment variables do not show.

mc admin config import[](#mc.admin.config.import)

Imports configuration settings exported using mc admin config export.

mc admin config reset[](#mc.admin.config.reset)

Resets config to defaults.
Configurations defined in environment variables are not affected.

mc admin config restore[](#mc.admin.config.restore)

Roll back changes to configuration keys to a previous point in history.

Does not affect configurations defined by environment variables.

## Configuration Settings

For a list of available configuration settings, see [[reference/minio-server/settings#minio-server-configuration-settings|Settings Overview]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).