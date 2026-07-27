---
title: "mc admin user svcacct edit — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-user-svcacct-edit.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-user-svcacct-edit"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin user svcacct edit — SILO (MinIO Object Storage)"
---
# mc admin user svcacct edit

Important

This command has been replaced and will be deprecated in a future MinIO Client release.

As of MinIO Client RELEASE.2024-10-08T09-37-26Z, use the [[reference/minio-mc-admin/mc-admin-accesskey-edit#command-mc.admin.accesskey.edit|`mc admin accesskey edit`]] command to modify access keys for built-in MinIO IDP users.

To modify access keys for AD/LDAP users, use the [[reference/minio-mc/mc-idp-ldap-accesskey-edit#command-mc.idp.ldap.accesskey.edit|`mc idp ldap accesskey edit`]] command.

## Syntax

The [`mc admin user svcacct edit`](#command-mc.admin.user.svcacct.edit) command modifies the configuration of an access key associated to the specified user.

EXAMPLE

The following command applies a new policy and secret key to the `myuserserviceaccount` access key on the `myminio` deployment:

```
mc admin user svcacct edit                                             \
                      --secret-key "myuserserviceaccountnewsecretkey"  \
                      --policy "/path/to/new/policy.json"              \
                      myminio myuserserviceaccount
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin user svcacct edit            \
                                    [--secret-key]  \
                                    [--policy]      \
                                    ALIAS           \
                                    SERVICEACCOUNT
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.user.svcacct.edit.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

SERVICEACCOUNT[](#mc.admin.user.svcacct.edit.SERVICEACCOUNT)
Required

The service account to modify.

--description[](#mc.admin.user.svcacct.edit.-description)
Optional

New in version RELEASE.2023-05-18T16-59-00Z.

Add a description for the service account.
For example, you might specify the reason the service account exists.

--expiry[](#mc.admin.user.svcacct.edit.-expiry)
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

--name[](#mc.admin.user.svcacct.edit.-name)
Optional

New in version RELEASE.2023-05-18T16-59-00Z.

Add a human-readable name for the service account.

--policy[](#mc.admin.user.svcacct.edit.-policy)
Optional

The path to a [[administration/identity-access-management/policy-based-access-control#minio-policy|policy document]] to attach to the new access key, with a maximum size of 2048 characters.
The attached policy cannot grant access to any action or resource not explicitly allowed by the parent user’s policies.

The new policy overwrites any previously attached policy.

--secret-key[](#mc.admin.user.svcacct.edit.-secret-key)
Optional

The secret key to associate with the new access key.
Overwrites the previous secret key.
Applications using the access keys must update to use the new credentials to continue performing operations.

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