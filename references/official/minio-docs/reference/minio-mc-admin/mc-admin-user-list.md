---
title: "mc admin user ls — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-user-list.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-user-list"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin user ls — SILO (MinIO Object Storage)"
---
# mc admin user ls

## Syntax

The [`mc admin user ls`](#command-mc.admin.user.ls) command lists all [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO users]] on the target MinIO deployment.

The [`mc admin user list`](#command-mc.admin.user.list) command has equivalent functionality to [`mc admin user ls`](#command-mc.admin.user.ls).

[`mc admin user ls`](#command-mc.admin.user.ls) does not return the access key or secret key associated to a user.
Use [[reference/minio-mc-admin/mc-admin-user-info#command-mc.admin.user.info|`mc admin user info`]] to retrieve detailed user information, including the user access key.

To manage external Identity Provider users, see [[reference/minio-mc/mc-idp-openid#command-mc.idp.openid|`OIDC`]] or [[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`AD/LDAP`]].

EXAMPLE

The following command lists all users on the `myminio` MinIO deployment:

```
mc admin user ls myminio
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin user list   \
                            ALIAS
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.user.ls.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment from which the command lists users.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Example

### List Available Users

Use [`mc admin user ls`](#command-mc.admin.user.ls) to list all users on a MinIO deployment:

```
mc admin user ls ALIAS
```

Replace [`ALIAS`](#mc.admin.user.ls.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

The output resembles the following:

```
enabled    devadmin              readwrite
enabled    devtest               readonly
enabled    newuser
```

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