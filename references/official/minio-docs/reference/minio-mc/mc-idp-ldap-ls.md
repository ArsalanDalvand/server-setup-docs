---
title: "mc idp ldap ls — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-idp-ldap-ls.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-idp-ldap-ls"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc idp ldap ls — SILO (MinIO Object Storage)"
---
# mc idp ldap ls

## Description

The [`mc idp ldap ls`](#command-mc.idp.ldap.ls) command lists the existing set of configurations for an AD/LDAP provider.

[`mc idp ldap ls`](#command-mc.idp.ldap.ls) is also known as [`mc idp ldap list`](#command-mc.idp.ldap.list).

EXAMPLE

The following example lists the AD/LDAP configuration settings for the `myminio` deployment.

```
mc idp ldap ls       \
            myminio
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp ldap ls     \
                          ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to list the AD/LDAP integration.

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.idp.ldap.list.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment for which to output the current AD/LDAP configuration.

For example:

```
mc idp ldap ls myminio
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