---
title: "mc admin user accesskey edit — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-accesskey-edit.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-accesskey-edit"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin user accesskey edit — SILO (MinIO Object Storage)"
---
# mc admin user accesskey edit

## Syntax

The [`mc admin accesskey edit`](#command-mc.admin.accesskey.edit) command modifies the configuration of an access key associated to the specified user.

The command requires that at least one attribute of the access key change.
Otherwise, the command exits with an error message.

EXAMPLE

The following command applies a new policy and secret key to the `myuserserviceaccount` access key on the `myminio` deployment:

```
mc admin accesskey edit                                             \
                   myminio myuserserviceaccount                     \
                   --secret-key "myuserserviceaccountnewsecretkey"  \
                   --policy "/path/to/new/policy.json"
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin accesskey edit                      \
                                 ALIAS                     \
                                 ACCESSKEY                 \
                                 [--description string]    \
                                 [--expiry-duration value] \
                                 [--expiry value]          \
                                 [--name string]           \
                                 [--policy path]           \
                                 [--secret-key string]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.accesskey.edit.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

ACCESSKEY[](#mc.admin.accesskey.edit.ACCESSKEY)
Required

The access key to modify.

--description[](#mc.admin.accesskey.edit.-description)
Optional

Add or modify a description for the access key.
For example, you might specify the reason the access key exists.

--expiry[](#mc.admin.accesskey.edit.-expiry)
Optional

Set or modify an expiration date for the access key.
The date must be in the future, you may not set an expiration date that has already passed.

Allowed date and time formats:

`2023-06-24`

`2023-06-24T10:00`

`2023-06-24T10:00:00`

`2023-06-24T10:00:00Z`

`2023-06-24T10:00:00-07:00`

Mutually exclusive with [`--expiry-duration`](#mc.admin.accesskey.edit.-expiry-duration).

--expiry-duration[](#mc.admin.accesskey.edit.-expiry-duration)
Optional

Length of time for which the accesskey remains valid.
Valid time units are “ns”, “us” (or “µs”), “ms”, “s”, “m”, “h”.

To expire the credentials after 30 days, use:

```
--expiry-duration 720h
```

Mutually exclusive with [`--expiry`](#mc.admin.accesskey.edit.-expiry).

--name[](#mc.admin.accesskey.edit.-name)
Optional

Add or modify a human-readable name for the access key.

--policy[](#mc.admin.accesskey.edit.-policy)
Optional

The path to a [[administration/identity-access-management/policy-based-access-control#minio-policy|policy document]] to attach to the new access key, with a maximum size of 2048 characters.
The attached policy cannot grant access to any action or resource not explicitly allowed by the parent user’s policies.

The new policy overwrites any previously attached policy.

--secret-key[](#mc.admin.accesskey.edit.-secret-key)
Optional

The secret key to associate with the new access key.
Overwrites the previous secret key.
Applications using the access keys must update to use the new credentials to continue performing operations.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Change the secret key for an access key

The following command modifies the secret key for the access key `myuseraccesskey` on the `myminio` deployment.

```
mc admin accesskey edit myminio/ myuseraccesskey --secret-key 'new-secret-key-change-me'
```

### Change the expiration for an access key

The following command changes the expiration value for the access key `myuseraccesskey` on the `myminio` deployment.

```
mc admin accesskey edit myminio/ myuseraccesskey --expiry-duration 24h
```

The [`--expiry-duration`](#mc.admin.accesskey.edit.-expiry-duration) cannot be added if the access key already has a value set for [`--expiry`](#mc.admin.accesskey.edit.-expiry).

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