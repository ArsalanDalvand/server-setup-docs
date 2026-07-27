---
title: "mc idp ldap accesskey info — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-idp-ldap-accesskey-info.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-idp-ldap-accesskey-info"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc idp ldap accesskey info — SILO (MinIO Object Storage)"
---
# mc idp ldap accesskey info

## Description

The [`mc idp ldap accesskey info`](#command-mc.idp.ldap.accesskey.info) outputs information about the specified access key(s).

This command works against [[administration/identity-access-management/minio-user-management#minio-id-access-keys|access keys]] created by an AD/LDAP user after authenticating to MinIO.

Create AD/LDAP service accounts with the [[reference/minio-mc/mc-idp-ldap-accesskey-create#command-mc.idp.ldap.accesskey.create|`mc idp ldap accesskey create`]] command.

MinIO supports using [[developers/security-token-service/AssumeRoleWithLDAPIdentity#minio-sts-assumerolewithldapidentity|AssumeRoleWithLDAPIdentity]] to generate temporary access keys using the [[developers/security-token-service#minio-security-token-service|Security Token Service]].

EXAMPLE

The following example outputs details for the access key `mykey` from the `minio` deployment:

```
mc idp ldap accesskey info minio/ mykey
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp ldap accesskey info      \
                                    ALIAS     \
                                    KEY       \
                                    [KEY2] ...
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment configured for AD/LDAP integration.

Replace `KEY` with the access key to delete.
You can list more than one access key by separating each key with a space.

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.idp.ldap.accesskey.info.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment configured for AD/LDAP.

For example:

```
mc idp ldap accesskey ls minio
```

KEY[](#mc.idp.ldap.accesskey.info.KEY)
Required

The configured access key to output information about.

You can list more than one access key by separating each key with a space.

### Example

Output information about the access keys `mykey` and `mykey2` from the `minio` deployment.

```
mc idp ldap accesskey info minio/ mykey mykey2
```

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