---
title: "mc admin accesskey — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-accesskey.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-accesskey"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin accesskey — SILO (MinIO Object Storage)"
---
# mc admin accesskey

New in version MinIO: Client RELEASE.2024-10-08T09-37-26Z

These commands replace the MinIO IDP functionality of the [[reference/minio-mc-admin/mc-admin-user-svcacct#command-mc.admin.user.svcacct|`mc admin user svcacct`]] command and its subcommands.

## Description

The [`mc admin accesskey`](#command-mc.admin.accesskey) command and its subcommands create and manage [[administration/identity-access-management/minio-user-management#minio-idp-service-account|Access Keys]] for internally managed users on a MinIO deployment.

Each access key is linked to a [[administration/identity-access-management#minio-authentication-and-identity-management|user identity]] and inherits the [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] attached to its parent user or those groups in which the parent user has membership.
Each access key also supports an optional inline policy which further restricts access to a subset of actions and resources available to the parent user.

[[reference/minio-mc-admin/mc-admin-user-svcacct#command-mc.admin.user.svcacct|`mc admin user svcacct`]] only supports creating access keys for [[administration/identity-access-management/minio-user-management#minio-users|MinIO-managed]] accounts.

To create access keys for [[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|Active Directory/LDAP-managed]] accounts, use [[reference/minio-mc/mc-idp-ldap-accesskey#command-mc.idp.ldap.accesskey|`mc idp ldap accesskey`]] and its subcommands.
To manage access keys for [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Connect-managed users]], log into the [[administration/minio-console#minio-console|MinIO Console]] and generate the access keys through the UI.

[`mc admin accesskey`](#command-mc.admin.accesskey) command has the following subcommands:

Subcommand

Description

[[reference/minio-mc-admin/mc-admin-accesskey-create#command-mc.admin.accesskey.create|`create`]]

The [[reference/minio-mc-admin/mc-admin-accesskey-create#command-mc.admin.accesskey.create|`mc admin accesskey create`]] command adds a new access key and secret key pair for an existing MinIO user.

[[reference/minio-mc-admin/mc-admin-accesskey-disable#command-mc.admin.accesskey.disable|`disable`]]

The [[reference/minio-mc-admin/mc-admin-accesskey-disable#command-mc.admin.accesskey.disable|`mc admin accesskey disable`]] command disables an existing access key for a MinIO IDP user.

[[reference/minio-mc-admin/mc-admin-accesskey-edit#command-mc.admin.accesskey.edit|`edit`]]

The [[reference/minio-mc-admin/mc-admin-accesskey-edit#command-mc.admin.accesskey.edit|`mc admin accesskey edit`]] command modifies the configuration of an access key associated to the specified user.

[[reference/minio-mc-admin/mc-admin-accesskey-enable#command-mc.admin.accesskey.enable|`enable`]]

The [[reference/minio-mc-admin/mc-admin-accesskey-enable#command-mc.admin.accesskey.enable|`mc admin accesskey enable`]] command enables an existing access key.

[[reference/minio-mc-admin/mc-admin-accesskey-info#command-mc.admin.accesskey.info|`info`]]

The [[reference/minio-mc-admin/mc-admin-accesskey-info#command-mc.admin.accesskey.info|`mc admin accesskey info`]] command returns a description of the specified [[administration/identity-access-management/minio-user-management#minio-id-access-keys|access key(s)]].

[[reference/minio-mc-admin/mc-admin-accesskey-list#command-mc.admin.accesskey.ls|`ls`]]

The [[reference/minio-mc-admin/mc-admin-accesskey-list#command-mc.admin.accesskey.ls|`mc admin accesskey ls`]] command lists users, access keys, or temporary [[developers/security-token-service#minio-security-token-service|security token service]] keys managed by the MinIO deployment.

[[reference/minio-mc-admin/mc-admin-accesskey-remove#command-mc.admin.accesskey.rm|`rm`]]

The [[reference/minio-mc-admin/mc-admin-accesskey-remove#command-mc.admin.accesskey.rm|`mc admin accesskey rm`]] command removes an access key associated to a user on the deployment.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).