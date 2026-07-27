---
title: "mc idp ldap rm — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-idp-ldap-rm.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-idp-ldap-rm"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc idp ldap rm — SILO (MinIO Object Storage)"
---
# mc idp ldap rm

## Description

The [`mc idp ldap rm`](#command-mc.idp.ldap.rm) command removes the existing configuration for an AD/LDAP provider.

[`mc idp ldap rm`](#command-mc.idp.ldap.rm) is also known as [`mc idp ldap remove`](#command-mc.idp.ldap.remove).

EXAMPLE

The following example removes the AD/LDAP provider settings for the `myminio` deployment.

```
mc idp ldap rm       \
            myminio
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp ldap rm     \
                          ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to remove the AD/LDAP integration.

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.idp.ldap.remove.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment for which to remove the current AD/LDAP configuration.

For example:

```
mc idp ldap rm myminio
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