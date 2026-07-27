---
title: "mc admin user rm — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-user-remove.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-user-remove"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin user rm — SILO (MinIO Object Storage)"
---
# mc admin user rm

## Syntax

The [`mc admin user rm`](#command-mc.admin.user.rm) command removes a [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO user]] on the target MinIO deployment.

The [`mc admin user remove`](#command-mc.admin.user.remove) command has equivalent functionality to [`mc admin user rm`](#command-mc.admin.user.rm).

To manage external Identity Provider users, see [[reference/minio-mc/mc-idp-openid#command-mc.idp.openid|`OIDC`]] or [[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`AD/LDAP`]].

EXAMPLE

The following command removes user `myuser` on the `myminio` MinIO deployment:

```
mc admin user rm myminio myuser
```

SYNTAX

Removes a user on the target MinIO deployment.

The command has the following syntax:

```
mc [GLOBALFLAGS] admin user remove    \
                            ALIAS     \
                            USERNAME
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.user.rm.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the configured MinIO deployment with the user to remove.

USERNAME[](#mc.admin.user.rm.USERNAME)
Required

The username of the user to remove.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Example

### Remove a User

Use [`mc admin user rm`](#command-mc.admin.user.rm) to remove a user from a MinIO deployment:

```
mc admin user rm ALIAS USERNAME
```

Replace [`ALIAS`](#mc.admin.user.rm.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`USERNAME`](#mc.admin.user.rm.USERNAME) with the username of the user to remove.

## Behavior

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).