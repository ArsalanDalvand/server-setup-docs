---
title: "mc admin user svcacct info — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-user-svcacct-info.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-user-svcacct-info"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin user svcacct info — SILO (MinIO Object Storage)"
---
# mc admin user svcacct info

Important

This command has been replaced and will be deprecated in a future MinIO Client release.

As of MinIO Client RELEASE.2024-10-08T09-37-26Z, use the [[reference/minio-mc-admin/mc-admin-accesskey-info#command-mc.admin.accesskey.info|`mc admin accesskey info`]] command to display information about access keys for built-in MinIO IDP users.

For access keys for AD/LDAP users, use the [[reference/minio-mc/mc-idp-ldap-accesskey-info#command-mc.idp.ldap.accesskey.info|`mc idp ldap accesskey info`]] command.

## Syntax

The [`mc admin user svcacct info`](#command-mc.admin.user.svcacct.info) command returns a description of the specified [[administration/identity-access-management/minio-user-management#minio-id-access-keys|access key]].

“Access Keys” have equivalent functionality to and replace the concept of “Service Accounts” in MinIO.

The description output includes the following details, as available:

Access Key

Parent user of the specified access key

Access key status (`on` or `off`)

Policy or policies

Comment

Expiration

Use [`--policy`](#mc.admin.user.svcacct.info.-policy) to view the attached policies.

EXAMPLE

The following command returns information on the specified access key:

```
mc admin user svcacct info myminio myuseraccesskey
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin user svcacct info           \
                                    [--policy]     \
                                    ALIAS          \
                                    ACCESSKEY
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.user.svcacct.info.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

ACCESSKEY[](#mc.admin.user.svcacct.info.ACCESSKEY)
Required

The service account access key to display.

--policy[](#mc.admin.user.svcacct.info.-policy)
Optional

Displays policies attached to the specified service account.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Display Service Account Details

Use [`mc admin user svcacct info`](#command-mc.admin.user.svcacct.info) to display details of a service account on a MinIO deployment:

```
mc admin user svcacct info ALIAS ACCESSKEY
```

Replace [[reference/minio-mc-admin/mc-admin-user-add#mc.admin.user.add.ALIAS|`ALIAS`]] with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`ACCESSKEY`](#mc.admin.user.svcacct.info.ACCESSKEY) with the service account access key.

The output resembles the following:

```
AccessKey: myuserserviceaccount
ParentUser: myuser
Status: on
Comment:
Policy: implied
Expiration: no-expiry
```

### Display Service Account Policy Details

Use [`mc admin user svcacct info`](#command-mc.admin.user.svcacct.info) to display the policies attached to service account:

```
mc admin user svcacct info --policy ALIAS ACCESSKEY
```

Replace [[reference/minio-mc-admin/mc-admin-user-add#mc.admin.user.add.ALIAS|`ALIAS`]] with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`ACCESSKEY`](#mc.admin.user.svcacct.info.ACCESSKEY) with the service account access key.

The output resembles the following:

```
{
 "Version": "2012-10-17",
 "Statement": [
  {
   "Effect": "Allow",
   "Action": [
    "s3:*"
   ],
   "Resource": [
    "arn:aws:s3:::*"
   ]
  }
 ]
}
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