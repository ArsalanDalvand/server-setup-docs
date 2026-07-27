---
title: "mc admin idp ldap policy — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-admin-idp-ldap-policy.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-admin-idp-ldap-policy"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin idp ldap policy — SILO (MinIO Object Storage)"
---
# mc admin idp ldap policy

Changed in version RELEASE.2023-05-26T23-31-54Z: `mc admin idp ldap policy` and its subcommands replaced by [[reference/minio-mc/mc-idp-ldap-policy#command-mc.idp.ldap.policy|`mc idp ldap policy`]].

## Description

The [`mc admin idp ldap policy`](#command-mc.admin.idp.ldap.policy) command allows you to view the mapping relationships between policies and the associated groups or users.

The [`mc admin idp ldap policy`](#command-mc.admin.idp.ldap.policy) command has the following subcommands:

Subcommand

Description

[`mc admin idp ldap policy attach`](#mc.admin.idp.ldap.policy.attach)

Attach a policy to an entity

[`mc admin idp ldap policy detach`](#mc.admin.idp.ldap.policy.detach)

Detach a policy from an entity

[`mc admin idp ldap policy entities`](#mc.admin.idp.ldap.policy.entities)

List policy entity mappings

## Syntax

attach[](#mc.admin.idp.ldap.policy.attach)

Attach one or more polices to entity.

EXAMPLES

The following example attaches two policies, `policy1` and `policy2`, to the `projectb` group on the `myminio` deployment.

```
mc admin idp ldap policy attach myminio/                                               \
                                 policy1                                                \
                                 policy2                                                \
                                 --group='cn=projectb,ou=groups,ou=swengg,dc=min,dc=io'
```

The following example attaches the policy, `userpolicy`, to the user `bobfisher` on the `myminio` deployment.

```
mc admin idp ldap policy attach myminio/                                               \
                                 mypolicy                                               \
                                 policy2                                                \
                                 --user='uid=bobfisher,ou=people,ou=hwengg,dc=min,dc=io'
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin idp ldap policy attach     \
                                POLICYNAME        \
                                [POLICY2] ...     \
                                ALIAS             \
                                [--user=`USER`]   \
                                [--group=`GROUP`]
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to configure for AD/LDAP integration.

Replace `POLICYNAME` with the policy to attach to the entity.
You may list multiple policies to attach to the entity.

Use must use one of either the `--user` or `--group` flag.
You may only use the flag once in the command.
You cannot use both flags in the same command.

detach[](#mc.admin.idp.ldap.policy.detach)

Detach one or more policies from an entity.

EXAMPLES

The following example detaches two policies, `policy1` and `policy2`, from the `projectb` group on the `myminio` deployment.

```
mc admin idp ldap policy detach myminio/                                               \
                                 policy1                                                \
                                 policy2                                                \
                                 --group='cn=projectb,ou=groups,ou=swengg,dc=min,dc=io'
```

The following example detaches the policy, `userpolicy`, from the user `bobfisher` on the `myminio` deployment.

```
mc admin idp ldap policy detach myminio/                                               \
                                 mypolicy                                               \
                                 policy2                                                \
                                 --user='uid=bobfisher,ou=people,ou=hwengg,dc=min,dc=io'
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin idp ldap policy detach     \
                                POLICYNAME        \
                                [POLICY2] ...     \
                                ALIAS             \
                                [--user=`USER`]   \
                                [--group=`GROUP`]
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to configure for AD/LDAP integration.

Replace `POLICYNAME` with the policy to detach from the entity.
You may list multiple policies to detach from the entity.

Use must use one of either the `--user` or `--group` flag.
You may only use the flag once in the command.
You cannot use both flags in the same command.

entities[](#mc.admin.idp.ldap.policy.entities)

Display a list of mappings for a user, group, and/or policy.

EXAMPLES

The following example lists all mappings for a specific policy, a set of groups, and a selection of users on the `myminio` deployment.

Specifically, it lists
- Users mapped to the `finteam-policy` policy.
- Policies assigned to the `uid=bobfisher,ou=people,ou=hwengg,dc=min,dc=io` user
- Policies assigned to the `cn=projectb,ou=groups,ou=swengg,dc=min,dc=io` group

```
mc admin idp ldap policy entities myminio/                                            \
                              --policy finteam-policy                                  \
                              --user 'uid=bobfisher,ou=people,ou=hwengg,dc=min,dc=io'  \
                              --group 'cn=projectb,ou=groups,ou=swengg,dc=min,dc=io'
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin idp ldap policy entities                \
                                ALIAS                          \
                                [--user `value`, -u `value`]   \
                                [--group `value`, -g `value`]  \
                                [--policy value]
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to configure for AD/LDAP integration.

You may use each of the `--user`, `--group`, and/or `--policy` flags as many times as desired in the command.

For each flag, the output lists the entities mapped to the specified policy, user, or group.

Omit all flags to return a list of mappings for all policies.

## Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).