---
title: "mc admin user svcacct rm — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-user-svcacct-remove.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-user-svcacct-remove"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin user svcacct rm — SILO (MinIO Object Storage)"
---
# mc admin user svcacct rm

Important

This command has been replaced and will be deprecated in a future MinIO Client release.

As of MinIO Client RELEASE.2024-10-08T09-37-26Z, use the [[reference/minio-mc-admin/mc-admin-accesskey-remove#command-mc.admin.accesskey.rm|`mc admin accesskey rm`]] command to delete access keys for built-in MinIO IDP users.

For access keys for AD/LDAP users, use the [[reference/minio-mc/mc-idp-ldap-accesskey-rm#command-mc.idp.ldap.accesskey.rm|`mc idp ldap accesskey rm`]] command.

## Syntax

The [`mc admin user svcacct rm`](#command-mc.admin.user.svcacct.rm) command removes an access key associated to a user on the deployment.

The [`mc admin user svcacct remove`](#command-mc.admin.user.svcacct.remove) command has equivalent functionality to [`mc admin user svcacct rm`](#command-mc.admin.user.svcacct.rm).

Applications can no longer authenticate using that access key after removal.

EXAMPLE

The following command removes the specified access key:

```
mc admin user svcacct rm myminio myuserserviceaccount
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin user svcacct remove          \
                                    ALIAS           \
                                    SERVICEACCOUNT
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.user.svcacct.rm.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

SERVICEACCOUNT[](#mc.admin.user.svcacct.rm.SERVICEACCOUNT)
Required

The service account access key to remove.

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