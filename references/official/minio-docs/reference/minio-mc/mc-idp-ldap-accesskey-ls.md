---
title: "mc idp ldap accesskey ls — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-idp-ldap-accesskey-ls.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-idp-ldap-accesskey-ls"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc idp ldap accesskey ls — SILO (MinIO Object Storage)"
---
# mc idp ldap accesskey ls

## Description

The [`mc idp ldap accesskey ls`](#command-mc.idp.ldap.accesskey.ls) displays a list of LDAP access key pairs.

[`mc idp ldap accesskey ls`](#command-mc.idp.ldap.accesskey.ls) is also known as [`mc idp ldap accesskey list`](#command-mc.idp.ldap.accesskey.list).

This command works against [[administration/identity-access-management/minio-user-management#minio-id-access-keys|access keys]] created by an AD/LDAP user after authenticating to MinIO.

Create AD/LDAP service accounts with the [[reference/minio-mc/mc-idp-ldap-accesskey-create#command-mc.idp.ldap.accesskey.create|`mc idp ldap accesskey create`]] command.

MinIO supports using [[developers/security-token-service/AssumeRoleWithLDAPIdentity#minio-sts-assumerolewithldapidentity|AssumeRoleWithLDAPIdentity]] to generate temporary access keys using the [[developers/security-token-service#minio-security-token-service|Security Token Service]].

EXAMPLE

The following example returns a list of access keys associated with the authenticated user on the `minio` [[reference/minio-mc/mc-alias-set#alias|alias]]:

```
mc idp ldap accesskey ls minio/
```

If the authenticated user has the `admin:ListUsers` permission, the example command returns a list of all users and their associated access keys.

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp ldap accesskey ls           \
                                 ALIAS           \
                                 [--all]         \
                                 [--self]        \
                                 [--svcacc-only] \
                                 [--temp-only]   \
                                 [--users-only]  \
                                 [DN] ...
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment configured for AD/LDAP integration.

Replace `DN` with the string of a user’s [distinguished name](https://learn.microsoft.com/en-us/previous-versions/windows/desktop/ldap/distinguished-names).
You may list multiple distinguished names by separating each with a space.

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.idp.ldap.accesskey.ls.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment configured for AD/LDAP.

For example:

```
mc idp ldap accesskey ls minio
```

--all[](#mc.idp.ldap.accesskey.ls.-all)
Optional

New in version mc: RELEASE.2024-07-31T15-58-33Z

List all access keys for all LDAP users.

--self[](#mc.idp.ldap.accesskey.ls.-self)
Optional

New in version mc: RELEASE.2024-07-31T15-58-33Z

List access keys for the currently authenticated user.

--svcacc-only[](#mc.idp.ldap.accesskey.ls.-svcacc-only)
Optional

Output only service account access keys.

Mutually exclusive with [`--temp-only`](#mc.idp.ldap.accesskey.ls.-temp-only).

--temp-only[](#mc.idp.ldap.accesskey.ls.-temp-only)
Optional

Output only temporary access keys.

Mutually exclusive with [`--svcacc-only`](#mc.idp.ldap.accesskey.ls.-svcacc-only).

--users-only[](#mc.idp.ldap.accesskey.ls.-users-only)
Optional

Output only the user distinguished names.

### Examples

#### List All Access Keys

To return a list of all access keys, you must first authenticate as the `admin` user.
Once authenticated, the following command returns all AD/LDAP access keys on the `minio` deployment.

```
mc idp ldap accesskey ls minio
```

Note

If the user does not have the `admin:ListUsers` permission, the command returns a list of access keys for the authenticated user only.

#### List User Distinguished Names

To return a list of DNs for a deployment, you must first authenticate as a user with the `admin:ListUsers` permission.
Once authenticated, the following command outputs the AD/LDAP distinguished names on the `minio` deployment.

```
mc idp ldap accesskey ls minio --users-only
```

#### List Temporary Access Keys

To return a list of all temporary access keys for a deployment, you must first authenticate as a user with the `admin:ListUsers` permission.
Once authenticated, the following command outputs a list of distinguished names with their associated temporary access keys.

```
mc idp ldap accesskey ls minio --temp-only
```

#### List a User’s Access Keys

The following command returns the AD/LDAP access keys for the user `bobfisher` on the `minio` deployment.

```
mc idp ldap accesskey list minio/ uid=bobfisher,dc=min,dc=io
```

#### List Access Keys for Multiple Users

The following command returns the AD/LDAP access keys for the users `bobfisher` and `cody3` on the `minio` deployment.

```
mc idp ldap accesskey list minio/ uid=bobfisher,dc=min,dc=io uid=cody3,dc=min,dc=io
```

#### List Access Keys for Authenticated User

The following command returns the AD/LDAP access keys for the currently authenticated user on the `minio` deployment.

```
mc idp ldap accesskey list minio/
```

Note

If the authenticated user has the `admin:ListUsers` permission, the command returns a list of all users and access keys on the deployment.

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