---
title: "mc admin user info — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-user-info.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-user-info"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin user info — SILO (MinIO Object Storage)"
---
# mc admin user info

## Syntax

The [`mc admin user info`](#command-mc.admin.user.info) command returns detailed information of a [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO user]] on the target MinIO deployment.

To manage external Identity Provider users, see [[reference/minio-mc/mc-idp-openid#command-mc.idp.openid|`OIDC`]] or [[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`AD/LDAP`]].

EXAMPLE

The following command returns details of user `myuser` on the `myminio` MinIO deployment:

```
mc admin user info myminio myuser
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin user info      \
                            ALIAS     \
                            USERNAME
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.user.info.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of a configured MinIO deployment to retrieve user information from.

USERNAME[](#mc.admin.user.info.USERNAME)

The username to retrieve information for.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

Changed in version RELEASE.2023-05-26T23-31-54Z: `mc admin user info --json` output includes policies inherited from a user’s group memberships in `memberOf`.

## Examples

### View User Details

Use [`mc admin user info`](#command-mc.admin.user.info) to view detailed user information for a user on a MinIO deployment:

```
mc admin user info ALIAS USERNAME
```

Replace [`ALIAS`](#mc.admin.user.info.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`USERNAME`](#mc.admin.user.info.USERNAME) with the username of the user to display information for.

For the [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO internal IDentity Provider (IDP)]], the output resembles the following:

```
AccessKey: miniouser
Status: enabled
PolicyName:
MemberOf: []
Authentication: builtin (miniouser)
```

For a [[operations/external-iam#minio-external-identity-management|third-party]] identity service such as LDAP, the output resembles the following:

```
AccessKey: uid=dillon,ou=people,ou=swengg,dc=min,dc=io
Status:
PolicyName: consoleAdmin
MemberOf: []
Authentication: ldap/localhost:1389 (uid=dillon,ou=people,ou=swengg,dc=min,dc=io)
```

### View Policies from Group Membership

Use [`mc admin user info`](#command-mc.admin.user.info) with :option::–json <mc.–json> to view the policies inherited from a user’s [[administration/identity-access-management/minio-group-management#minio-groups|group memberships]]:

```
mc admin user info ALIAS USERNAME --json
```

Replace [`ALIAS`](#mc.admin.user.info.ALIAS) with the [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [`USERNAME`](#mc.admin.user.info.USERNAME) with the username of the user to display information for.

The `memberOf` property in the output contains a list of groups the user is a member of, with the policies attached to each group.
The output resembles the following:

```
{
 "status": "success",
 "accessKey": "myuser",
 "userStatus": "enabled",
 "memberOf": [
  {
   "name": "testingGroup",
   "policies": [
    "testingGroupPolicy"
   ]
 "authentication": builtin (myuser)
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