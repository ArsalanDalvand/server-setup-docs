---
title: "mc admin accesskey info — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-accesskey-info.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-accesskey-info"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin accesskey info — SILO (MinIO Object Storage)"
---
# mc admin accesskey info

## Syntax

The [`mc admin accesskey info`](#command-mc.admin.accesskey.info) command returns a description of the specified [[administration/identity-access-management/minio-user-management#minio-id-access-keys|access key(s)]].

The description output includes the following details, as available:

Access Key

Parent user of the specified access key

Access key status (`on` or `off`)

Policy or policies

Comment

Expiration

EXAMPLE

The following command returns information on the specified access key:

```
mc admin accesskey info myminio myuseraccesskey
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin accesskey info      \
                                 ALIAS     \
                                 ACCESSKEY
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.accesskey.info.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

ACCESSKEY[](#mc.admin.accesskey.info.ACCESSKEY)
Required

The access key to display.

Return information for multiple access keys by separating each access key with a space.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Display access key details

Use [`mc admin accesskey info`](#command-mc.admin.accesskey.info) to display details of an access key on a MinIO deployment:

```
mc admin accesskey info myminio myaccesskey
```

Replace `myminio` with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [[reference/minio-mc-admin/mc-admin-user-svcacct-info#mc.admin.user.svcacct.info.ACCESSKEY|`myaccesskey`]] with the access key for which to display information.
List multiple keys by separating each with a space.

The output resembles the following:

```
AccessKey: myuserserviceaccount
ParentUser: myuser
Status: on
Comment:
Policy: implied
Expiration: no-expiry
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