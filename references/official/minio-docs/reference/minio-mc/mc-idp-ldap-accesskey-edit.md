---
title: "mc idp ldap accesskey edit — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-idp-ldap-accesskey-edit.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-idp-ldap-accesskey-edit"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc idp ldap accesskey edit — SILO (MinIO Object Storage)"
---
# mc idp ldap accesskey edit

## Description

[`mc idp ldap accesskey edit`](#command-mc.idp.ldap.accesskey.edit) modifies the specified [[administration/identity-access-management/minio-user-management#minio-id-access-keys|access key]] on the local server.

EXAMPLE

The following example modifies the secret for the access key `mykey` on the `minio` deployment:

```
mc idp ldap accesskey edit myminio/ mykey --secret-key 'xxxxxxx'
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp ldap accesskey rm                        \
                                 ALIAS                        \
                                 KEY                          \
                                 [--secret-key <string>]      \
                                 [--policy <string>]          \
                                 [--name <string>]            \
                                 [--description <string>]     \
                                 [--expiry-duration <string>] \
                                 [--expiry <string>]
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment configured for AD/LDAP integration.

Replace `KEY` with the access key to delete.

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.idp.ldap.accesskey.edit.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment configured for AD/LDAP.

For example:

```
mc idp ldap accesskey ls minio
```

KEY[](#mc.idp.ldap.accesskey.edit.KEY)
Required

The configured access key to delete.

--description[](#mc.idp.ldap.accesskey.edit.-description)
Optional

Add a description for the service account.
For example, you might specify the reason the access key exists.

--expiry[](#mc.idp.ldap.accesskey.edit.-expiry)
Optional

The date after which the access key expires.
Enter the date in YYYY-MM-DD format.

For example, to expire the credentials after December 31, 2024, enter `2024-12-31`.

Mutually exclusive with [`--expiry-duration`](#mc.idp.ldap.accesskey.edit.-expiry-duration).

--expiry-duration[](#mc.idp.ldap.accesskey.edit.-expiry-duration)
Optional

Length of time the access key pair should remain valid for use in `#d#h#s` format.

For example, `7d`, `24h`, `5d12h30s` are valid strings.

Mutually exclusive with [`--expiry`](#mc.idp.ldap.accesskey.edit.-expiry).

--name[](#mc.idp.ldap.accesskey.edit.-name)
Optional

A human-readable name to use for the account.

--policy[](#mc.idp.ldap.accesskey.edit.-policy)
Optional

File path to the JSON-formatted policy to use for the account.

If not specified, the account uses the same policy as the authenticated user.

--secret-key[](#mc.idp.ldap.accesskey.edit.-secret-key)
Optional

A secret to use for the account.

### Example

#### Modify a secret for an access key

Modify the secret for the access key `mykey` on the `minio` deployment.

```
mc idp ldap accesskey edit myminio/ mykey --secret-key 'xxxxxxx'
```

#### Modify the expiration duration for an accesskey

Modify the expiration duration for the access key `mykey` on the `minio` deployment.

```
mc idp ldap accesskey edit myminio/ mykey ---expiry-duration 24h
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