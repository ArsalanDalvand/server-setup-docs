---
title: "mc admin user add — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-user-add.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-user-add"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin user add — SILO (MinIO Object Storage)"
---
# mc admin user add

## Syntax

The [`mc admin user add`](#command-mc.admin.user.add) command adds a new [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO user]] to the target MinIO deployment.

To manage external Identity Provider users, see [[reference/minio-mc/mc-idp-openid#command-mc.idp.openid|`OIDC`]] or [[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`AD/LDAP`]].

EXAMPLE

The following command creates a new user `newuser` on the `myminio` MinIO deployment:

```
mc admin user add myminio newuser newusersecret
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin user add        \
                            ALIAS      \
                            ACCESSKEY  \
                            SECRETKEY
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ACCESSKEY[](#mc.admin.user.add.ACCESSKEY)
Required

The access key that uniquely identifies the new user, similar to a username.

ALIAS[](#mc.admin.user.add.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment on which to create the new user.

SECRETKEY[](#mc.admin.user.add.SECRETKEY)
Required

The secret key for the new user. Consider the following guidance when creating a secret key:

The key should be unique

The key should be long (Greater than 12 characters)

The key should be complex (A mixture of characters, numerals, and symbols)

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Example

### Create a New User

Use [`mc admin user add`](#command-mc.admin.user.add) to create a user on a MinIO deployment:

```
mc admin user add ALIAS ACCESSKEY SECRETKEY
```

Replace [`ALIAS`](#mc.admin.user.add.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`ACCESSKEY`](#mc.admin.user.add.ACCESSKEY) with the access key for the user.

Replace [`SECRETKEY`](#mc.admin.user.add.SECRETKEY) with the secret key for the user.
MinIO does not provide any method for retrieving the secret key once set.

Specify a unique, random, and long string for both the `ACCESSKEY` and `SECRETKEY`.
Your organization may have specific internal or regulatory requirements around generating values for use with access or secret keys.

## Behavior

### New Users Have No Default Policies

Newly created users have no policies by default and therefore cannot perform any operations on the MinIO deployment.
To configure a user’s assigned policies, you can do either or both of the following:

Use [[reference/minio-mc-admin/mc-admin-policy-attach#command-mc.admin.policy.attach|`mc admin policy attach`]] to associate one or more policies to the user.

Use [[reference/minio-mc-admin/mc-admin-group#mc.admin.group.add|`mc admin group add`]] to associate the user to the group.
Users inherit any policies assigned to the group.

For more information on MinIO users and groups, see [[administration/identity-access-management/minio-user-management#minio-users|User Management]] and [[administration/identity-access-management/minio-group-management#minio-groups|Group Management]].
For more information on MinIO policies, see [[administration/identity-access-management/policy-based-access-control#minio-policy|MinIO Policy Based Access Control]].

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