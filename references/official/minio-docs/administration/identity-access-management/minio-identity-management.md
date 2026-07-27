---
title: "MinIO Identity Management — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/identity-access-management/minio-identity-management.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/identity-access-management/minio-identity-management"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO Identity Management — SILO (MinIO Object Storage)"
---
# MinIO Identity Management

MinIO includes a built-in IDentity Provider (IDP) that provides core identity
management functionality. The MinIO IDP supports creating an arbitrary number of
long-lived users on the deployment for supporting client authentication.

Each user consists of a unique access key (username) and corresponding secret
key (password). Clients must authenticate their identity by specifying both
a valid access key (username) and the corresponding secret key (password) of
an existing MinIO user.

Administrators use the [[reference/minio-mc-admin/mc-admin-user#command-mc.admin.user|`mc admin user`]] command to create and manage
MinIO users.

MinIO also supports creating [[administration/identity-access-management/minio-user-management#minio-idp-service-account|access keys]]. Access Keys are child identities of an
authenticated parent user and inherit their permissions from the parent.

MinIO by default denies access to all actions or resources not explicitly
allowed by a user’s assigned or inherited [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]]. You
must either explicitly assign a [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]] describing the
user’s authorized actions and resources or assign the user to [[administration/identity-access-management/minio-group-management#minio-groups|groups]] which have associated policies. See
[[administration/identity-access-management#minio-access-management|Access Management]] for more information.

External Identity Management

MinIO supports external management of identities using either an
OpenID Connect (OIDC) or Active Directory/LDAP IDentity Provider (IDP).
For more information, see:

[[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Connect Access Management]]

[[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|Active Directory / LDAP Access Management]]

AD/LDAP and OIDC configurations are mutually exclusive.
Furthermore, enabling AD/LDAP external identity management disables the MinIO internal IDP, with the exception of creating [[administration/identity-access-management/minio-user-management#minio-idp-service-account|access keys]].
You can configure multiple OIDC providers while maintaining MinIO-managed users.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).