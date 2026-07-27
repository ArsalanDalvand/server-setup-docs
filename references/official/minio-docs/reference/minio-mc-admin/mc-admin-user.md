---
title: "mc admin user — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc-admin/mc-admin-user.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc-admin/mc-admin-user"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc admin user — SILO (MinIO Object Storage)"
---
# mc admin user

## Description

The [`mc admin user`](#command-mc.admin.user) command and its subcommands manage [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO users]].

Clients must authenticate to the MinIO deployment with the access key and secret key associated to a user on the deployment.
MinIO users constitute a key component in MinIO Identity and Access Management.

To manage users who authenticate using a 3rd party IDP, use the command for the appropriate provider:

For AD/LDAP, use [[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`mc idp ldap`]]

For OpenID Connect (OIDC) compatible providers, use [[reference/minio-mc/mc-idp-openid#command-mc.idp.openid|`mc idp openid`]]

Use `mc idp` commands on MinIO Deployments Only

[[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`mc idp ldap`]] and [[reference/minio-mc/mc-idp-openid#command-mc.idp.openid|`mc idp openid`]] and their subcommands are only supported against MinIO deployments.

## Subcommands

[`mc admin user`](#command-mc.admin.user) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc-admin/mc-admin-user-add#command-mc.admin.user.add|`add`]]

The [[reference/minio-mc-admin/mc-admin-user-add#command-mc.admin.user.add|`mc admin user add`]] command adds a new [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO user]] to the target MinIO deployment.

[[reference/minio-mc-admin/mc-admin-user-disable#command-mc.admin.user.disable|`disable`]]

The [[reference/minio-mc-admin/mc-admin-user-disable#command-mc.admin.user.disable|`mc admin user disable`]] command disables a [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO user]] on the target MinIO deployment.

[[reference/minio-mc-admin/mc-admin-user-enable#command-mc.admin.user.enable|`enable`]]

The [[reference/minio-mc-admin/mc-admin-user-enable#command-mc.admin.user.enable|`mc admin user enable`]] command enables a [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO user]] on the target MinIO deployment.

[[reference/minio-mc-admin/mc-admin-user-info#command-mc.admin.user.info|`info`]]

The [[reference/minio-mc-admin/mc-admin-user-info#command-mc.admin.user.info|`mc admin user info`]] command returns detailed information of a [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO user]] on the target MinIO deployment.

[[reference/minio-mc-admin/mc-admin-user-list#command-mc.admin.user.ls|`ls`]]

The [[reference/minio-mc-admin/mc-admin-user-list#command-mc.admin.user.ls|`mc admin user ls`]] command lists all [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO users]] on the target MinIO deployment.

[[reference/minio-mc-admin/mc-admin-user-remove#command-mc.admin.user.rm|`rm`]]

The [[reference/minio-mc-admin/mc-admin-user-remove#command-mc.admin.user.rm|`mc admin user rm`]] command removes a [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO user]] on the target MinIO deployment.

[[reference/minio-mc-admin/mc-admin-user-sts-info#command-mc.admin.user.sts.info|`sts info`]]

The [[reference/minio-mc-admin/mc-admin-user-sts-info#command-mc.admin.user.sts.info|`mc admin user sts info`]] command retrieves information on the specified STS credential, such as the parent [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO user]] who generated the credentials, associated policies, and expiration.

[[reference/minio-mc-admin/mc-admin-user-svcacct#command-mc.admin.user.svcacct|`svcacct`]]

The [[reference/minio-mc-admin/mc-admin-user-svcacct#command-mc.admin.user.svcacct|`mc admin user svcacct`]] command and its subcommands create and manage [[administration/identity-access-management/minio-user-management#minio-idp-service-account|Access Keys]] on a MinIO deployment.

As of MinIO Client RELEASE.2024-10-08T09-37-26Z, these commands have been replaced by [[reference/minio-mc-admin/mc-admin-accesskey#command-mc.admin.accesskey|`mc admin accesskey`]] and [[reference/minio-mc/mc-idp-ldap-accesskey#command-mc.idp.ldap.accesskey|`mc idp ldap accesskey`]].
This command and its subcommands will be deprecated in a future MinIO Client release.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).