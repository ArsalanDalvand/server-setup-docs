---
title: "mc idp ldap accesskey — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-idp-ldap-accesskey.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-idp-ldap-accesskey"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc idp ldap accesskey — SILO (MinIO Object Storage)"
---
# mc idp ldap accesskey

New in version RELEASE.2023-10-30T18-43-32Z.

## Description

The [`mc idp ldap accesskey`](#command-mc.idp.ldap.accesskey) commands allow you to list, delete, or display information about LDAP access key pairs.

The [`mc idp ldap accesskey`](#command-mc.idp.ldap.accesskey) commands are only supported against MinIO deployments.

This command works against [[administration/identity-access-management/minio-user-management#minio-id-access-keys|access keys]] created by an AD/LDAP user after authenticating to MinIO.

Create AD/LDAP service accounts with the [[reference/minio-mc/mc-idp-ldap-accesskey-create#command-mc.idp.ldap.accesskey.create|`mc idp ldap accesskey create`]] command.

MinIO supports using [[developers/security-token-service/AssumeRoleWithLDAPIdentity#minio-sts-assumerolewithldapidentity|AssumeRoleWithLDAPIdentity]] to generate temporary access keys using the [[developers/security-token-service#minio-security-token-service|Security Token Service]].

The [`mc idp ldap accesskey`](#command-mc.idp.ldap.accesskey) command has the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-idp-ldap-accesskey-create#command-mc.idp.ldap.accesskey.create|`mc idp ldap accesskey create`]]

The [[reference/minio-mc/mc-idp-ldap-accesskey-create#command-mc.idp.ldap.accesskey.create|`mc idp ldap accesskey create`]] allows you to add LDAP access key pairs.

[[reference/minio-mc/mc-idp-ldap-accesskey-disable#command-mc.idp.ldap.accesskey.disable|`mc idp ldap accesskey disable`]]

[[reference/minio-mc/mc-idp-ldap-accesskey-disable#command-mc.idp.ldap.accesskey.disable|`mc idp ldap accesskey disable`]] disables the specified [[administration/identity-access-management/minio-user-management#minio-id-access-keys|access key]] on the MinIO deployment.

[[reference/minio-mc/mc-idp-ldap-accesskey-edit#command-mc.idp.ldap.accesskey.edit|`mc idp ldap accesskey edit`]]

[[reference/minio-mc/mc-idp-ldap-accesskey-edit#command-mc.idp.ldap.accesskey.edit|`mc idp ldap accesskey edit`]] modifies the specified [[administration/identity-access-management/minio-user-management#minio-id-access-keys|access key]] on the local server.

[[reference/minio-mc/mc-idp-ldap-accesskey-enable#command-mc.idp.ldap.accesskey.enable|`mc idp ldap accesskey enable`]]

The [[reference/minio-mc/mc-idp-ldap-accesskey-enable#command-mc.idp.ldap.accesskey.enable|`mc idp ldap accesskey enable`]] enables the specified [[administration/identity-access-management/minio-user-management#minio-id-access-keys|access key]] on the local server.

[[reference/minio-mc/mc-idp-ldap-accesskey-info#command-mc.idp.ldap.accesskey.info|`mc idp ldap accesskey info`]]

The [[reference/minio-mc/mc-idp-ldap-accesskey-info#command-mc.idp.ldap.accesskey.info|`mc idp ldap accesskey info`]] outputs information about the specified access key(s).

[[reference/minio-mc/mc-idp-ldap-accesskey-ls#command-mc.idp.ldap.accesskey.ls|`mc idp ldap accesskey ls`]]

The [[reference/minio-mc/mc-idp-ldap-accesskey-ls#command-mc.idp.ldap.accesskey.ls|`mc idp ldap accesskey ls`]] displays a list of LDAP access key pairs.

[[reference/minio-mc/mc-idp-ldap-accesskey-rm#command-mc.idp.ldap.accesskey.rm|`mc idp ldap accesskey rm`]]

The [[reference/minio-mc/mc-idp-ldap-accesskey-rm#command-mc.idp.ldap.accesskey.rm|`mc idp ldap accesskey rm`]] deletes the specified access key from the local server.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).