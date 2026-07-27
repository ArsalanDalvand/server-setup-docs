---
title: "mc admin user svcacct — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-user-svcacct.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-user-svcacct"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin user svcacct — SILO (MinIO Object Storage)"
---
# mc admin user svcacct

Important

These commands have been replaced and will be deprecated in a future MinIO Client release.

As of MinIO Client RELEASE.2024-10-08T09-37-26Z, use the [[reference/minio-mc-admin/mc-admin-accesskey#command-mc.admin.accesskey|`mc admin accesskey`]] command and its subcommands for functions related to built-in MinIO IDP users and their access keys or STS tokens.

For access keys for AD/LDAP users, use the [[reference/minio-mc/mc-idp-ldap-accesskey#command-mc.idp.ldap.accesskey|`mc idp ldap accesskey`]] command and its subcommands.

## Description

The [`mc admin user svcacct`](#command-mc.admin.user.svcacct) command and its subcommands create and manage [[administration/identity-access-management/minio-user-management#minio-idp-service-account|Access Keys]] on a MinIO deployment.

As of MinIO Client RELEASE.2024-10-08T09-37-26Z, these commands have been replaced by [[reference/minio-mc-admin/mc-admin-accesskey#command-mc.admin.accesskey|`mc admin accesskey`]] and [[reference/minio-mc/mc-idp-ldap-accesskey#command-mc.idp.ldap.accesskey|`mc idp ldap accesskey`]].
This command and its subcommands will be deprecated in a future MinIO Client release.

Each access key is linked to a [[administration/identity-access-management#minio-authentication-and-identity-management|user identity]] and inherits the [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] attached to its parent user or those groups in which the parent user has membership.
Each access key also supports an optional inline policy which further restricts access to a subset of actions and resources available to the parent user.

[`mc admin user svcacct`](#command-mc.admin.user.svcacct) only supports creating access keys for [[administration/identity-access-management/minio-user-management#minio-users|MinIO-managed]] and [[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|Active Directory/LDAP-managed]] accounts.

To create access keys for [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Connect-managed users]], log into the [[administration/minio-console#minio-console|MinIO Console]] and generate the access keys through the UI.

The [`mc admin user svcacct`](#command-mc.admin.user.svcacct) command has the following subcommands:

Subcommand

Description

[[reference/minio-mc-admin/mc-admin-user-svcacct-add#command-mc.admin.user.svcacct.add|`add`]]

The [[reference/minio-mc-admin/mc-admin-user-svcacct-add#command-mc.admin.user.svcacct.add|`mc admin user svcacct add`]] command adds a new access key to an existing MinIO or AD/LDAP user.

[[reference/minio-mc-admin/mc-admin-user-svcacct-disable#command-mc.admin.user.svcacct.disable|`disable`]]

The [[reference/minio-mc-admin/mc-admin-user-svcacct-disable#command-mc.admin.user.svcacct.disable|`mc admin user svcacct disable`]] command disables an existing access key.

[[reference/minio-mc-admin/mc-admin-user-svcacct-edit#command-mc.admin.user.svcacct.edit|`edit`]]

The [[reference/minio-mc-admin/mc-admin-user-svcacct-edit#command-mc.admin.user.svcacct.edit|`mc admin user svcacct edit`]] command modifies the configuration of an access key associated to the specified user.

[[reference/minio-mc-admin/mc-admin-user-svcacct-enable#command-mc.admin.user.svcacct.enable|`enable`]]

The [[reference/minio-mc-admin/mc-admin-user-svcacct-enable#command-mc.admin.user.svcacct.enable|`mc admin user svcacct enable`]] command enables an existing access key.

[[reference/minio-mc-admin/mc-admin-user-svcacct-info#command-mc.admin.user.svcacct.info|`info`]]

The [[reference/minio-mc-admin/mc-admin-user-svcacct-info#command-mc.admin.user.svcacct.info|`mc admin user svcacct info`]] command returns a description of the specified [[administration/identity-access-management/minio-user-management#minio-id-access-keys|access key]].

[[reference/minio-mc-admin/mc-admin-user-svcacct-list#command-mc.admin.user.svcacct.list|`list`]]

The [[reference/minio-mc-admin/mc-admin-user-svcacct-list#command-mc.admin.user.svcacct.ls|`mc admin user svcacct ls`]] command lists all access keys associated to the specified user.

[[reference/minio-mc-admin/mc-admin-user-svcacct-remove#command-mc.admin.user.svcacct.rm|`rm`]]

The [[reference/minio-mc-admin/mc-admin-user-svcacct-remove#command-mc.admin.user.svcacct.rm|`mc admin user svcacct rm`]] command removes an access key associated to a user on the deployment.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).