---
title: "mc admin idp ldap — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/deprecated/mc-admin-idp-ldap.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/deprecated/mc-admin-idp-ldap"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin idp ldap — SILO (MinIO Object Storage)"
---
# mc admin idp ldap

Changed in version RELEASE.2023-05-26T23-31-54Z: `mc admin idp ldap` and its subcommands replaced by [[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`mc idp ldap`]].

## Description

The [`mc admin idp ldap`](#command-mc.admin.idp.ldap) commands allow you to add, modify, review, list, remove, enable, and disable server configurations to 3rd party [[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|Active Directory or LDAP Identity and Access Management (IAM) integrations]].

Define configuration settings as an alternative to using environment variables when [[operations/external-iam/configure-ad-ldap-external-identity-management#minio-authenticate-using-ad-ldap-generic|setting up an AD/LDAP connection]].

Note

Configuration settings do not override settings configured as environment variables.

The [`mc admin idp ldap`](#command-mc.admin.idp.ldap) command has the following subcommands:

Subcommand

Description

[`mc admin idp ldap add`](#mc.admin.idp.ldap.add)

Create an AD/LDAP IDP server configuration.

[`mc admin idp ldap update`](#mc.admin.idp.ldap.update)

Modify an existing AD/LDAP IDP server configuration.

[`mc admin idp ldap ls`](#mc.admin.idp.ldap.ls)

Lists AD/LDAP server configurations.

[`mc admin idp ldap rm`](#mc.admin.idp.ldap.rm)

Remove an AD/LDAP IDP server configuration from a deployment.

[`mc admin idp ldap info`](#mc.admin.idp.ldap.info)

Displays details for a specific AD/LDAP server configuration.

[`mc admin idp ldap enable`](#mc.admin.idp.ldap.enable)

Enables an AD/LDAP server configuration.

[`mc admin idp ldap disable`](#mc.admin.idp.ldap.disable)

Disables an AD/LDAP server configuration.

[[reference/deprecated/mc-admin-idp-ldap-policy#mc.admin.idp.ldap.policy.entities|`mc admin idp ldap policy entities`]]

List policy association entities

## Configuration Parameters

The [`mc admin idp ldap`](#command-mc.admin.idp.ldap) subcommands support configuration parameters.
The parameters define the server’s interaction with the Active Directory or LDAP IAM provider.

For a more detailed explanation of the configuration parameters, refer to the [[reference/minio-server/settings/iam/ldap#minio-ldap-config-settings|config setting documentation]].

## Syntax

add[](#mc.admin.idp.ldap.add)

Create a new configuration for an AD/LDAP provider.
MinIO supports no more than one (1) AD/LDAP provider per deployment.

EXAMPLE

The following example sets the AD/LDAP configuration settings for the `myminio` deployment.

```
mc admin idp ldap add                                               \
      myminio                                                        \
      server_addr=myldapserver:636                                   \
      lookup_bind_dn=cn=admin,dc=min,dc=io                           \
      lookup_bind_password=somesecret                                \
      user_dn_search_base_dn=dc=min,dc=io                            \
      user_dn_search_filter="(uid=%s)"                               \
      group_search_base_dn=ou=swengg,dc=min,dc=io                    \
      group_search_filter="(&(objectclass=groupofnames)(member=%d))"
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin idp ldap add          \
                           ALIAS             \
                           [CFG_PARAM1]      \
                           [CFG_PARAM2]...
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to create for AD/LDAP integration.

Replace the `[CFG_PARAM#]` with each of the [[reference/minio-server/settings/iam/ldap#minio-ldap-config-settings|configuration setting]] key-value pairs in the format of `PARAMETER="value"`.

update[](#mc.admin.idp.ldap.update)

Modify an existing set of configurations for an AD/LDAP provider.

EXAMPLE

The following example changes two of the AD/LDAP configuration settings for the `myminio` deployment.

```
mc admin idp ldap update                                \
                  myminio                               \
                  lookup_bind_dn=cn=admin,dc=min,dc=io  \
                  lookup_bind_password=somesecret
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin idp ldap update           \
                                ALIAS            \
                                [CFG_PARAM1]     \
                                [CFG_PARAM2]...
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to update for AD/LDAP integration.

Replace the `[CFG_PARAM#]` with each of the [[reference/minio-server/settings/iam/ldap#minio-ldap-config-settings|configuration setting]] key-value pairs to update in the format of `PARAMETER="value"`.

ls, list[](#mc.admin.idp.ldap.ls)

Lists the existing set of configurations for an AD/LDAP provider.

EXAMPLE

The following example lists the AD/LDAP configuration settings for the `myminio` deployment.

```
mc admin idp ldap ls myminio
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin idp ldap ls ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to list the AD/LDAP integration.

rm, remove[](#mc.admin.idp.ldap.rm)

Remove the existing configuration for an AD/LDAP provider.

EXAMPLE

The following example removes the AD/LDAP provider settings for the `myminio` deployment.

```
mc admin idp ldap rm myminio
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin idp ldap rm     \
                                ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to remove the AD/LDAP integration.

info[](#mc.admin.idp.ldap.info)

Outputs the current configuration for an AD/LDAP provider on a specified MinIO deployment.

EXAMPLE

The following example outputs the AD/LDAP configuration settings on the `myminio` deployment.

```
mc admin idp ldap info myminio
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin idp ldap info     \
                                ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to retrieve info on the AD/LDAP integration.

enable[](#mc.admin.idp.ldap.enable)

Enables the currently configured AD/LDAP provider.

EXAMPLE

The following example enables the AD/LDAP configuration on the `myminio` deployment.

```
mc admin idp ldap enable       \
                  myminio
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin idp ldap enable     \
                                ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to enable the AD/LDAP integration.

disable[](#mc.admin.idp.ldap.disable)

Disables the currently configured AD/LDAP provider.

EXAMPLE

The following example disables the AD/LDAP configurations on the `myminio` deployment.

```
mc admin idp ldap disable      \
                  myminio
```

SYNTAX

The command has the following syntax:

```
mc [GLOBALFLAGS] admin idp ldap disable       \
                                ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of a MinIO deployment to disable the AD/LDAP integration.

## Global Flags

This command supports any of the [[reference/minio-mc#minio-mc-global-options|global flags]].

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).