---
title: "mc idp ldap accesskey create-with-login — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-idp-ldap-accesskey-create-with-login.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-idp-ldap-accesskey-create-with-login"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc idp ldap accesskey create-with-login — SILO (MinIO Object Storage)"
---
# mc idp ldap accesskey create-with-login

New in version mc: RELEASE.2024-04-18T16-45-29Z

## Description

The [`mc idp ldap accesskey create-with-login`](#command-mc.idp.ldap.accesskey.create-with-login) uses interactive terminal-based prompt to authenticate with the external AD/LDAP server and generate access keys for use with MinIO.

EXAMPLE

The following example prompts the user to provide their AD/LDAP credentials.
It then generates a new access key pair using the policy or policies associated with that AD/LDAP user.

```
mc idp ldap accesskey create-with-login https://minio.example.net/
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp ldap accesskey create-with-login        \
                                 URL                         \
                                 [--access-key <value>]      \
                                 [--secret-key <value>]      \
                                 [--policy <value>]          \
                                 [--name <value>]            \
                                 [--description <value>]     \
                                 [--expiry <value>]          \
                                 [--expiry-duration <value>]
```

Replace `URL` with the FQDN of a MinIO deployment configured for AD/LDAP integration.

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

URL[](#mc.idp.ldap.accesskey.create-with-login.URL)
Required

The FQDN of a MinIO deployment configured for AD/LDAP integration.

For example:

```
mc idp ldap accesskey create-with-login https://minio.example.net
```

--access-key[](#mc.idp.ldap.accesskey.create-with-login.-access-key)
Optional

The access key to use once successfully authenticated.
Omit to let MinIO randomly generate a value.

The access key cannot contain the characters `=` (equal sign) or `,` (comma).

Requires [`--secret-key`](#mc.idp.ldap.accesskey.create-with-login.-secret-key)

--secret-key[](#mc.idp.ldap.accesskey.create-with-login.-secret-key)
Optional

A secret key to use once successfully authenticated.
Omit to let MinIO randomly generate a value.

Requires [`--access-key`](#mc.idp.ldap.accesskey.create-with-login.-access-key)

--policy[](#mc.idp.ldap.accesskey.create-with-login.-policy)
Optional

File path to the JSON-formatted [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]] to use for the account.
This policy _cannot_ grant additional privileges beyond the privileges associated with the authenticated AD/LDAP user.

Omit to use the AD/LDAP user policies.

--name[](#mc.idp.ldap.accesskey.create-with-login.-name)
Optional

A human-readable name to use for the created access key.

--description[](#mc.idp.ldap.accesskey.create-with-login.-description)
Optional

Create a description for the service account.
For example, you might specify the reason the access key exists.

--expiry-duration[](#mc.idp.ldap.accesskey.create-with-login.-expiry-duration)
Optional

Length of time the access key pair should remain valid for use in `#d#h#s` format.

For example, `7d`, `24h`, `5d12h30s` are valid strings.

Mutually exclusive with [`--expiry`](#mc.idp.ldap.accesskey.create-with-login.-expiry).

--expiry[](#mc.idp.ldap.accesskey.create-with-login.-expiry)
Optional

The date after which the access key expires.
Enter the date in `YYYY-MM-DD` format.

For example, to expire the credentials after December 31, 2024, enter `2024-12-31`.

Mutually exclusive with [`--expiry-duration`](#mc.idp.ldap.accesskey.create-with-login.-expiry-duration).

### Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

## Behavior

### S3 Compatibility

The mc commandline tool is built for compatibility with the AWS S3
API and is tested with MinIO and AWS S3 for expected functionality and behavior.

MinIO provides no guarantees for other S3-compatible services, as their S3 API
implementation is unknown and therefore unsupported. While mc
commands may work as documented, any such usage is at your own risk.

## Examples

### Create a new access-key pair for the authenticated user

The following command creates a new access key pair to use with the currently authenticated user on the `minio` alias.
The command outputs a randomly generated access key and secret key.

```
mc idp ldap accesskey create-with-login https://minio.example.net
```

### Create a new access-key pair with a custom access key and secret key

The following command creates a new access key pair with both an access key and secret key that you specify for the user currently authenticated on the `minio` alias.

```
mc idp ldap accesskey create-with-login https://minio.example.net/ --access-key my-access-key-change-me --secret-key my-secret-key-change-me
```

### Create a new access-key pair that expires after 24 hours

The following command creates a new access key pair to use with the currently authenticated user on the `minio` alias.
The credentials expire after 24 hours.

The command outputs a randomly generated access key and secret key.

```
mc idp ldap accesskey create-with-login https://minio.example.net --expiry-duration 24h
```

### Create a new access-key pair that expires after a date

The following command creates a new access key pair to use with the currently authenticated user on the `minio` alias.
The credentials expire after February 28, 2025.

The command outputs a randomly generated access key and secret key.

```
mc idp ldap accesskey create-with-login https://minio.example.net --expiry 2025-02-28
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).