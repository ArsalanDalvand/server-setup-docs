---
title: "mc idp ldap — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-idp-ldap.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-idp-ldap"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc idp ldap — SILO (MinIO Object Storage)"
---
# mc idp ldap

New in version RELEASE.2023-05-26T23-31-54Z: [`mc idp ldap`](#command-mc.idp.ldap) and its subcommands replace `mc admin idp ldap`.

## Description

The [`mc idp ldap`](#command-mc.idp.ldap) commands allow you to manage configurations to 3rd party [[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|Active Directory or LDAP Identity and Access Management (IAM) integrations]].

The [`mc idp ldap`](#command-mc.idp.ldap) commands are an alternative to using environment variables when [[operations/external-iam/configure-ad-ldap-external-identity-management#minio-authenticate-using-ad-ldap-generic|setting up an AD/LDAP connection]]. They are only supported against MinIO deployments.

See [[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|Active Directory / LDAP Access Management]] for a tutorial on using these commands.

Note

MinIO [[reference/minio-server/settings/iam/ldap#minio-server-envvar-external-identity-management-ad-ldap|AD/LDAP environment variables]] override their corresponding configuration settings as modified or set by this command.

The [`mc idp ldap`](#command-mc.idp.ldap) command has the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-idp-ldap-add#command-mc.idp.ldap.add|`mc idp ldap add`]]

The [[reference/minio-mc/mc-idp-ldap-add#command-mc.idp.ldap.add|`mc idp ldap add`]] command creates an AD/LDAP IDP server configuration.

[[reference/minio-mc/mc-idp-ldap-disable#command-mc.idp.ldap.disable|`mc idp ldap disable`]]

The [[reference/minio-mc/mc-idp-ldap-disable#command-mc.idp.ldap.disable|`mc idp ldap disable`]] command disables the currently configured AD/LDAP provider.

[[reference/minio-mc/mc-idp-ldap-enable#command-mc.idp.ldap.enable|`mc idp ldap enable`]]

The [[reference/minio-mc/mc-idp-ldap-enable#command-mc.idp.ldap.enable|`mc idp ldap enable`]] command enables the currently configured AD/LDAP provider.

[[reference/minio-mc/mc-idp-ldap-info#command-mc.idp.ldap.info|`mc idp ldap info`]]

The [[reference/minio-mc/mc-idp-ldap-info#command-mc.idp.ldap.info|`mc idp ldap info`]] command outputs the current configuration for an AD/LDAP provider on a specified MinIO deployment.

[[reference/minio-mc/mc-idp-ldap-ls#command-mc.idp.ldap.ls|`mc idp ldap ls`]]

The [[reference/minio-mc/mc-idp-ldap-ls#command-mc.idp.ldap.ls|`mc idp ldap ls`]] command lists the existing set of configurations for an AD/LDAP provider.

[[reference/minio-mc/mc-idp-ldap-policy#command-mc.idp.ldap.policy|`mc idp ldap policy`]] subcommands

The [[reference/minio-mc/mc-idp-ldap-policy#command-mc.idp.ldap.policy|`mc idp ldap policy`]] commands show the mapping relationships between policies and the associated groups or users.

[[reference/minio-mc/mc-idp-ldap-rm#command-mc.idp.ldap.rm|`mc idp ldap rm`]]

The [[reference/minio-mc/mc-idp-ldap-rm#command-mc.idp.ldap.rm|`mc idp ldap rm`]] command removes the existing configuration for an AD/LDAP provider.

[[reference/minio-mc/mc-idp-ldap-update#command-mc.idp.ldap.update|`mc idp ldap update`]]

The [[reference/minio-mc/mc-idp-ldap-update#command-mc.idp.ldap.update|`mc idp ldap update`]] command modifies an existing set of configurations for an AD/LDAP provider.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).