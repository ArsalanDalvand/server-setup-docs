---
title: "mc admin update — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-update.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-update"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin update — SILO (MinIO Object Storage)"
---
# mc admin update

## Description

The [`mc admin update`](#command-mc.admin.update) command updates all MinIO servers in the deployment.
The command also supports using a private mirror server for environments where the deployment does not have public internet access.

After running the command, a prompt displays to confirm the update.
Type `y` and `[ENTER]` to confirm and proceed with the update.

The user must have `write` permissions for the target location where the binary installs.

Use `mc admin` on MinIO Deployments Only

MinIO does not support using [[reference/minio-mc-admin#command-mc.admin|`mc admin`]] commands with other
S3-compatible services, regardless of their claimed compatibility with MinIO
deployments.

## Considerations

### Updates are Non-Disruptive

[`mc admin update`](#command-mc.admin.update) updates the binary and restarts all MinIO servers in the deployment simultaneously.
MinIO operations are atomic and strictly consistent and as such the restart process is non-disruptive to applications.

MinIO strongly recommends only performing simultaneous upgrade-and-restart procedures.
Do not perform “rolling” (that is, one node at a time) upgrade procedures.

### Permissions

The user running the command must have `write` permissions to the target path where the MinIO Server binary installs.

## Examples

Use [`mc admin update`](#command-mc.admin.update) to update each [[reference/minio-server/minio-server#command-minio|`minio`]] server process in the MinIO deployment:

```
mc admin update ALIAS
```

Replace [`ALIAS`](#mc.admin.update.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

After running the command, answer yes to the prompt to confirm and process the update.

## Syntax

[`mc admin update`](#command-mc.admin.update) has the following syntax:

```
mc admin update ALIAS         \
                [MIRROR_URL]  \
                [--yes]
```

[`mc admin update`](#command-mc.admin.update) supports the following arguments:

ALIAS[](#mc.admin.update.ALIAS)

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment to update.

If the specified `ALIAS` corresponds to a distributed MinIO deployment, [`mc admin update`](#command-mc.admin.update) updates all MinIO servers in the deployment at the same time.

Use [[reference/minio-mc/mc-alias-list#command-mc.alias.list|`mc alias list`]] to review the configured aliases and their corresponding MinIO deployment endpoints.

MIRROR_URL[](#mc.admin.update.MIRROR_URL)

The mirror URL of the `minio` server binary to use for updating MinIO servers in the [`ALIAS`](#mc.admin.update.ALIAS) deployment.

--yes, -y[](#mc.admin.update.-yes)
Optional

Pass this flag to confirm the update and bypass the confirmation prompt.

## Behavior

### Binary Compression

Changed in version RELEASE.2024-01-28T22-35-53Z: [`mc admin update`](#command-mc.admin.update) compresses the binary before sending to all nodes in the deployment.

This feature does not apply to [[operations/deployments/baremetal#minio-baremetal|systemctl managed deployments]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).