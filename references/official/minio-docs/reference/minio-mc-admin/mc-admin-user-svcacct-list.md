---
title: "mc admin user svcacct ls — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-user-svcacct-list.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-user-svcacct-list"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin user svcacct ls — SILO (MinIO Object Storage)"
---
# mc admin user svcacct ls

Important

This command has been replaced and will be deprecated in a future MinIO Client release.

As of MinIO Client RELEASE.2024-10-08T09-37-26Z, use the [[reference/minio-mc-admin/mc-admin-accesskey-list#command-mc.admin.accesskey.ls|`mc admin accesskey ls`]] command to list access keys for built-in MinIO IDP users.

For access keys for AD/LDAP users, use the [[reference/minio-mc/mc-idp-ldap-accesskey-ls#command-mc.idp.ldap.accesskey.ls|`mc idp ldap accesskey ls`]] command.

## Syntax

The [`mc admin user svcacct ls`](#command-mc.admin.user.svcacct.ls) command lists all access keys associated to the specified user.

The alias [`mc admin user svcacct list`](#command-mc.admin.user.svcacct.list) has equivalent functionality to [`mc admin user svcacct ls`](#command-mc.admin.user.svcacct.ls).

EXAMPLE

The following command lists all access keys associated to the user with username `admin1`:

```
mc admin user svcacct ls myminio admin1
```

The output resembles the following:

```
Access Key        | Expiry
5XF3ZHNZK6FBDWH9JMLX | 2023-06-24 07:00:00 +0000 UTC
F4V2BBUZSWY7UG96ED70 | 2023-12-24 18:00:00 +0000 UTC
FZVSEZ8NM9JRBEQZ7B8Q | no-expiry
HOXGL8ON3RG0IKYCHCUD | no-expiry
```

New in version RELEASE.2023-05-26T23-31-54Z: The list of access keys includes the expiry date, or `no-expiry` for keys that do not expire.

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin user svcacct ls   \
                                    ALIAS  \
                                    USER
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.user.svcacct.ls.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

USER[](#mc.admin.user.svcacct.ls.USER)
Required

The username of the user to display access keys for.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

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