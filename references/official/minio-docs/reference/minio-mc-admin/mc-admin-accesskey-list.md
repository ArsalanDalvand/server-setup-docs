---
title: "mc admin accesskey ls — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-accesskey-list.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-accesskey-list"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin accesskey ls — SILO (MinIO Object Storage)"
---
# mc admin accesskey ls

## Syntax

The [`mc admin accesskey ls`](#command-mc.admin.accesskey.ls) command lists users, access keys, or temporary [[developers/security-token-service#minio-security-token-service|security token service]] keys managed by the MinIO deployment.

The alias [`mc admin accesskey list`](#command-mc.admin.accesskey.list) has equivalent functionality to [`mc admin accesskey ls`](#command-mc.admin.accesskey.ls).

EXAMPLE

The following command lists all access keys associated to the user with username `admin1` on the deployment at alias `myminio`:

```
mc admin accesskey ls myminio admin1
```

The output resembles the following:

```
Access Key        | Expiry
5XF3ZHNZK6FBDWH9JMLX | 2023-06-24 07:00:00 +0000 UTC
F4V2BBUZSWY7UG96ED70 | 2023-12-24 18:00:00 +0000 UTC
FZVSEZ8NM9JRBEQZ7B8Q | no-expiry
HOXGL8ON3RG0IKYCHCUD | no-expiry
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin accesskey ls             \
                                 ALIAS          \
                                 [USER]         \
                                 [--all]        \
                                 [--self]       \
                                 [--temp-only]  \
                                 [--users-only]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.accesskey.ls.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

USER[](#mc.admin.accesskey.ls.USER)
Optional

The username of the user(s) to display access keys for.
Separate multiple usernames with a space.

--all[](#mc.admin.accesskey.ls.-all)
Optional

List all users and any access keys or temporary STS keys associated with them.
Requires admin privileges for the deployment.

This flag is mutually exclusive with the other flags available for this command.

--svcacc-only[](#mc.admin.accesskey.ls.-svcacc-only)
Optional

List temporary [[developers/security-token-service#minio-security-token-service|Security Token Service (STS) keys]] on the deployment.

This flag is mutually exclusive with the other flags available for this command.

--self[](#mc.admin.accesskey.ls.-self)
Optional

List access keys and STS keys for the currently authenticated user.

This flag is mutually exclusive with the other flags available for this command.

--temp-only[](#mc.admin.accesskey.ls.-temp-only)
Optional

List users with their access keys.
This returns only users that have associated access keys.

This flag requires admin privileges for the user running the command.

This flag is mutually exclusive with the other flags available for this command.

--users-only[](#mc.admin.accesskey.ls.-users-only)
Optional

List the MinIO users managed by the deployment.
Use in conjunction with the [`--all`](#mc.admin.accesskey.ls.-all) flag to list all users on the deployment.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### List all built-in users and associated access keys

The following command lists all users managed by the MinIO deployment at alias `myminio` and any associated access keys or temporary STS tokens.

```
mc admin accesskey list myminio/ --all
```

### Return a list of access keys for the current authenticated user

The following command lists the access keys or temporary STS tokens associated with the currently authenticated user for the `myminio` deployment.

```
mc admin accesskey list myminio/ --self
```

### List all users created and managed by the deployment

The following command returns a list of all of the users on the current deployment.
The list only includes MinIO IDP managed users, not users managed by a third party tool on a protocol like OpenID or Active Directory/LDAP.

```
mc admin accesskey ls myminio/ --all --users-only
```

### Return a list of access keys associated with the users miniouser1 and miniouser2

The following command returns a list of access keys for two users on the `myminio` deployment.

```
mc admin accesskey ls myminio/ miniouser1 miniouser2
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