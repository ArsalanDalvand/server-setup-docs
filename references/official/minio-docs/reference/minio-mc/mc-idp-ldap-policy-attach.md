---
title: "mc idp ldap policy attach — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-idp-ldap-policy-attach.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-idp-ldap-policy-attach"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc idp ldap policy attach — SILO (MinIO Object Storage)"
---
# mc idp ldap policy attach

## Description

The [`mc idp ldap policy attach`](#command-mc.idp.ldap.policy.attach) command attaches one or more polices to an entity.

EXAMPLE

The following example attaches the policy `userpolicy` to the user `bobfisher` on the `myminio` deployment:

```
mc idp ldap policy attach myminio                                                  \
                          userpolicy                                               \
                          --user='uid=bobfisher,ou=people,ou=hwengg,dc=min,dc=io'
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] idp ldap policy attach             \
                                 POLICYNAME         \
                                 [POLICY2] ...      \
                                 ALIAS              \
                                 [--user=`USER`]    \
                                 [--group=`GROUP`]
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to configure for AD/LDAP integration.

Replace `POLICYNAME` with the policy to attach to the entity.
You may list multiple policies to attach to the entity.

Use must use one of either the `--user` or `--group` flag.
You may only use the flag once in the command.
You cannot use both flags in the same command.

Brackets `[]` indicate optional parameters.

Parameters sharing a line are mutually dependent.

Parameters separated using the pipe `|` operator are mutually exclusive.

Copy the example to a text editor and modify as-needed before running the command in the terminal/shell.

### Parameters

ALIAS[](#mc.idp.ldap.policy.attach.ALIAS)
Required

The [[reference/minio-mc/mc-alias-set#alias|alias]] of the MinIO deployment with the entity to which to attach a policy.

For example:

```
mc idp ldap policy attach myminio                                                  \
                          userpolicy                                               \
                          --user='uid=bobfisher,ou=people,ou=hwengg,dc=min,dc=io'
```

### Example

The following example attaches two policies, `policy1` and `policy2`, to the `projectb` group on the `myminio` deployment:

```
mc idp ldap policy attach myminio                                                 \
                          policy1                                                 \
                          policy2                                                 \
                          --group='cn=projectb,ou=groups,ou=swengg,dc=min,dc=io'
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