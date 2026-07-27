---
title: "mc admin user svcacct add — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-user-svcacct-add.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-user-svcacct-add"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin user svcacct add — SILO (MinIO Object Storage)"
---
# mc admin user svcacct add

Important

This command has been replaced and will be deprecated in a future MinIO Client release.

As of MinIO Client RELEASE.2024-10-08T09-37-26Z, use the [[reference/minio-mc-admin/mc-admin-accesskey-create#command-mc.admin.accesskey.create|`mc admin accesskey create`]] command to add access keys for built-in MinIO IDP users.

To add access keys for AD/LDAP users, use the [[reference/minio-mc/mc-idp-ldap-accesskey-create#command-mc.idp.ldap.accesskey.create|`mc idp ldap accesskey create`]] command.

## Syntax

The [`mc admin user svcacct add`](#command-mc.admin.user.svcacct.add) command adds a new access key to an existing MinIO or AD/LDAP user.

Access keys for OpenID Connect users

To generate service account access keys for [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Connect users]], use the [[administration/minio-console#minio-console|MinIO Console]].

EXAMPLE

The following command creates a new access key associated to an existing MinIO user:

```
mc admin user svcacct add                       \
   --access-key "myuserserviceaccount"          \
   --secret-key "myuserserviceaccountpassword"  \
   --policy "/path/to/policy.json"              \
   myminio myuser
```

The command returns the access key and secret key for the new account.

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin user svcacct add             \
                                    [--access-key]  \
                                    [--secret-key]  \
                                    [--policy]      \
                                    [--comment]     \
                                    ALIAS           \
                                    USER
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.user.svcacct.add.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

USER[](#mc.admin.user.svcacct.add.USER)
Required

The username of the user to which MinIO adds the new access key.

For [[administration/identity-access-management/minio-user-management#minio-users|MinIO-managed users]], specify the access key for the user.

For [[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|Active Directory/LDAP users]], specify the Distinguished Name of the user.

For [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Connect users]], use the [[administration/minio-console#minio-console|MinIO Console]] to generate access keys.

--access-key[](#mc.admin.user.svcacct.add.-access-key)
Optional

A string to use as the access key for this account.
Omit to let MinIO autogenerate a random 20 character value.

Access Key names must be unique across all users.

--comment[](#mc.admin.user.svcacct.add.-comment)
Optional

Changed in version RELEASE.2023-05-18T16-59-00Z: Replaced by [`--description`](#mc.admin.user.svcacct.add.-description) and [`--name`](#mc.admin.user.svcacct.add.-name).

Originally added in version RELEASE.2023-01-28T20-29-38Z.

This option has been removed.
Use `--description` or `--name` instead.

--description[](#mc.admin.user.svcacct.add.-description)
Optional

New in version RELEASE.2023-05-18T16-59-00Z.

Add a description for the service account.
For example, you might specify the reason the service account exists.

--expiry[](#mc.admin.user.svcacct.add.-expiry)
Optional

New in version RELEASE.2023-05-30T22-41-38Z.

Set an expiration date for the service account.
The date must be in the future, you may not set an expiration date that has already passed.

Allowed date and time formats:

`2023-06-24`

`2023-06-24T10:00`

`2023-06-24T10:00:00`

`2023-06-24T10:00:00Z`

`2023-06-24T10:00:00-07:00`

--name[](#mc.admin.user.svcacct.add.-name)
Optional

New in version RELEASE.2023-05-18T16-59-00Z.

Add a human-readable name for the service account.

--policy[](#mc.admin.user.svcacct.add.-policy)
Optional

The path to a [[administration/identity-access-management/policy-based-access-control#minio-policy|policy document]] to attach to the new access key, with a maximum size of 2048 characters.
The attached policy cannot grant access to any action or resource not explicitly allowed by the parent user’s policies.

--secret-key[](#mc.admin.user.svcacct.add.-secret-key)
Optional

The secret key to associate with the new account.
Omit to let MinIO autogenerate a random 40-character value.

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