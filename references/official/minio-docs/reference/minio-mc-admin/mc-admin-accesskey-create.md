---
title: "mc admin accesskey create — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-accesskey-create.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-accesskey-create"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin accesskey create — SILO (MinIO Object Storage)"
---
# mc admin accesskey create

## Syntax

The [`mc admin accesskey create`](#command-mc.admin.accesskey.create) command adds a new access key and secret key pair for an existing MinIO user.

Access keys for OpenID Connect or AD/LDAP users

This command is for access keys for users created directly on the MinIO deployment and not managed by a third party solution.

To generate access keys for [[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|Active Directory/LDAP users]], use [[reference/minio-mc/mc-idp-ldap-accesskey-create#command-mc.idp.ldap.accesskey.create|`mc idp ldap accesskey create`]].

EXAMPLE

The following command creates a new access key associated to an existing MinIO user:

```
mc admin accesskey create        \
   myminio/ myuser               \
   --access-key myuseraccesskey  \
   --secret-key myusersecretkey  \
   --policy /path/to/policy.json
```

The command returns the access key and secret key for the new account.

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin accesskey create                    \
                                 ALIAS                     \
                                 [USER]                    \
                                 [--access-key string]     \
                                 [--secret-key string]     \
                                 [--policy path]           \
                                 [--name string]           \
                                 [--description string]    \
                                 [--expiry-duration value] \
                                 [--expiry date]
```

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.admin.accesskey.create.ALIAS)
Required

The [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

USER[](#mc.admin.accesskey.create.USER)
Optional

The username of the user to which MinIO adds the new access key.
If not specified, MinIO generates an access key/secret key pair for the authenticated user.

--access-key[](#mc.admin.accesskey.create.-access-key)
Optional

A string to use as the access key for this account.
Omit to let MinIO autogenerate a random 20 character value.

Access Key names must be unique across all users.

--description[](#mc.admin.accesskey.create.-description)
Optional

Add a description for the access key.
For example, you might specify the reason the access key exists.

--expiry[](#mc.admin.accesskey.create.-expiry)
Optional

Set an expiration date for the access key.
The date must be in the future.
You may not set an expiration date that has already passed.

Allowed date and time formats:

`2024-10-24`

`2024-10-24T10:00`

`2024-10-24T10:00:00`

`2024-10-24T10:00:00Z`

`2024-10-24T10:00:00-07:00`

Mutually exclusive with [`--expiry-duration`](#mc.admin.accesskey.create.-expiry-duration).

--expiry-duration[](#mc.admin.accesskey.create.-expiry-duration)
Optional

Length of time for which the accesskey remains valid.
Valid time units are “ns”, “us” (or “µs”), “ms”, “s”, “m”, “h”.

The following expires the credentials after 30 days:

```
--expiry-duration 720h
```

Mutually exclusive with [`--expiry`](#mc.admin.accesskey.create.-expiry).

--name[](#mc.admin.accesskey.create.-name)
Optional

Add a human-readable name for the access key.

--policy[](#mc.admin.accesskey.create.-policy)
Optional

The readable path to a [[administration/identity-access-management/policy-based-access-control#minio-policy|policy document]] to attach to the new access key, with a maximum size of 2048 characters.
The attached policy cannot grant access to any action or resource not explicitly allowed by the parent user’s policy or group policies

--secret-key[](#mc.admin.accesskey.create.-secret-key)
Optional

The secret key to associate with the new account.
Omit to let MinIO autogenerate a random 40-character value.

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Examples

### Create access key / secret key pair for the authenticated user

The following command generates a new, random access key and secret key pair for the user currently logged in to MinIO deployment at the alias `myminio`.
The access key and secret key have the same access policies as the authenticated user.

```
mc admin accesskey create myminio/
```

### Create a custom access key / secret key pair for the authenticated user

The following command creates a new access key and secret key pair for the user currently logged in to MinIO at the alias `myminio`.
The access key and secret key have the same access policies as the authenticated user.

```
mc admin accesskey create myminio/ --access-key myaccesskey --secret-key mysecretkey
```

### Create an access key / secret key pair for another user with limited duration

The following command creates a new access key and secret key pair for a user, `miniouser` on the alias `myminio`.
The access key and secret key have the same access policies as `miniouser`.
The credentials remain valid for 24 hours after creation.

```
mc admin accesskey create myminio/ miniouser --expiry-duration 24h
```

### Create access key / secret key pair for the authenticated user that expires

The following command generates a new and random access key and random secret key pair for the user currently logged in to MinIO deployment at the alias `myminio`.
The access key and secret key have the same access policies as the authenticated user.
The credentials expire on the fifteenth day of January, 2025.

```
mc admin accesskey create myminio/ --expiry 2025-01-15
```

The date specified must be a future date.
For valid datetime formats, see the [`--expiry`](#mc.admin.accesskey.create.-expiry) flag.

### Create access key / secret key pair for a different user with custom access

The following command creates a new access key and secret key pair for the user, `miniouser` on the alias `myminio`.
The access key and secret key have a more limited set of access than `miniouser`, as specified in the policy JSON file.

```
mc admin accesskey create myminio/ miniouser --policy /path/to/policy.json
```

The specified policy file must not grant access to anything to which `miniouser` does not already have access.

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