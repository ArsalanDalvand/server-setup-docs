---
title: "mc admin user svcacct disable — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-user-svcacct-disable.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-user-svcacct-disable"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin user svcacct disable — SILO (MinIO Object Storage)"
---
# mc admin user svcacct disable

Important

This command has been replaced and will be deprecated in a future MinIO Client release.

As of MinIO Client RELEASE.2024-10-08T09-37-26Z, use the [[reference/minio-mc-admin/mc-admin-accesskey-disable#command-mc.admin.accesskey.disable|`mc admin accesskey disable`]] command to disable an access key for a built-in MinIO IDP user.

To disable access keys for AD/LDAP users, use the [[reference/minio-mc/mc-idp-ldap-accesskey-disable#command-mc.idp.ldap.accesskey.disable|`mc idp ldap accesskey disable`]] command.

## Syntax

The [`mc admin user svcacct disable`](#command-mc.admin.user.svcacct.disable) command disables an existing access key.

EXAMPLE

The following command disables the specified access key:

```
mc admin user svcacct disable myminio myuserserviceaccount
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin user svcacct disable         \
                                    ALIAS           \
                                    SERVICEACCOUNT
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.user.svcacct.disable.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

SERVICEACCOUNT[](#mc.admin.user.svcacct.disable.SERVICEACCOUNT)
Required

The service account access key to disable.

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