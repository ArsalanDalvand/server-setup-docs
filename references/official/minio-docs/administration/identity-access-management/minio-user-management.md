---
title: "User Management — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/identity-access-management/minio-user-management.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/identity-access-management/minio-user-management"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "User Management — SILO (MinIO Object Storage)"
---
# User Management

## Overview

A MinIO user consists of a unique access key (username) and corresponding secret
key (password). Clients must authenticate their identity by specifying both
a valid access key (username) and the corresponding secret key (password) of
an existing MinIO user.

Each user can have one or more assigned [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] that
explicitly list the actions and resources to which that user has access.
Users can also inherit policies from the [[administration/identity-access-management/minio-group-management#minio-groups|groups]] in which
they have membership.

MinIO by default denies access to all actions or resources not explicitly
allowed by a user’s assigned or inherited [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]]. You
must either explicitly assign a [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]] describing the
user’s authorized actions and resources or assign the user to [[administration/identity-access-management/minio-group-management#minio-groups|groups]] which have associated policies. See
[[administration/identity-access-management#minio-access-management|Access Management]] for more information.

This page documents user management for the MinIO internal IDentity Provider
(IDP). MinIO also external management of identities using either an
OpenID Connect (OIDC) or Active Directory/LDAP IDentity Provider (IDP).
For more information, see:

[[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Connect Access Management]]

[[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|Active Directory / LDAP Access Management]]

Enabling external identity management disables the MinIO internal IDP, with
the exception of creating [access keys](#minio-idp-service-account).

## Access Keys

MinIO Access Keys (formerly “Service Accounts”) are child identities of an authenticated MinIO user, including [[administration/identity-access-management#minio-authentication-and-identity-management|externally managed identities]].
Each access key inherits its privileges based on the [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] attached to it’s parent user or those groups in which the parent user has membership.
Access keys also support an optional inline policy which further restricts access to a subset of actions and resources available to the parent user.

A MinIO user can generate any number of access keys.
This allows application owners to generate arbitrary access keys for their applications without requiring action from the MinIO administrators.
Since the generated access keys have the same or fewer permissions as the parents, administrators can focus on managing the top-level parent users without micro-managing generated access keys.

You can create access keys by using the [[reference/minio-mc-admin/mc-admin-user-svcacct-add#command-mc.admin.user.svcacct.add|`mc admin user svcacct add`]] command.
Identities created by these methods do not expire until you remove the access key or the parent account.

You can also create [[developers/security-token-service#minio-security-token-service|security token service]] accounts programmatically with the `AssumeRole` STS API endpoint.
STS tokens default to expire in 1 hour, but you set expiration for up to 7 days from creation.

Access Keys are for Programmatic Access

Access Keys support programmatic access by applications.
You cannot use an access key to log into the MinIO Console.

## MinIO root User

MinIO deployments have a `root` user with access to all actions and resources
on the deployment, regardless of the configured [[administration/identity-access-management#minio-authentication-and-identity-management|identity manager]]. When a [[reference/minio-server/minio-server#command-minio|`minio`]] server first
starts, it sets the `root` user credentials by checking the value of the
following environment variables:

[[reference/minio-server/settings/root-credentials#envvar.MINIO_ROOT_USER|`MINIO_ROOT_USER`]]

[[reference/minio-server/settings/root-credentials#envvar.MINIO_ROOT_PASSWORD|`MINIO_ROOT_PASSWORD`]]

Rotating the root user credentials requires updating either or both variables
for all MinIO servers in the deployment. Specify long, unique, and random
strings for root credentials. Exercise all possible precautions in storing the
access key and secret key, such that only known and trusted individuals who
require superuser access to the deployment can retrieve the `root`
credentials.

MinIO strongly discourages using the `root` user for regular client access
regardless of the environment (development, staging, or production).

MinIO strongly recommends creating users such that each client has access to
the minimal set of actions and resources required to perform their assigned
workloads.

If these variables are unset, [[reference/minio-server/minio-server#command-minio|`minio`]] defaults to `minioadmin` and
`minioadmin` as the access key and secret key respectively. MinIO strongly
discourages use of the default credentials regardless of deployment
environment.

Deprecation of Legacy Root User Environment Variables

MinIO [RELEASE.2021-04-22T15-44-28Z](https://github.com/minio/minio/releases/tag/RELEASE.2021-04-22T15-44-28Z) and later deprecates the
following variables used for setting or updating root user
credentials:

[[reference/minio-server/settings/deprecated#envvar.MINIO_ACCESS_KEY|`MINIO_ACCESS_KEY`]] to the new access key.

[[reference/minio-server/settings/deprecated#envvar.MINIO_SECRET_KEY|`MINIO_SECRET_KEY`]] to the new secret key.

[[reference/minio-server/settings/deprecated#envvar.MINIO_ACCESS_KEY_OLD|`MINIO_ACCESS_KEY_OLD`]] to the old access key.

[[reference/minio-server/settings/deprecated#envvar.MINIO_SECRET_KEY_OLD|`MINIO_SECRET_KEY_OLD`]] to the old secret key.

## User Management

### Create a User

Use the [[reference/minio-mc-admin/mc-admin-user-add#command-mc.admin.user.add|`mc admin user add`]] command to create a new user on the
MinIO deployment:

```
mc admin user add ALIAS ACCESSKEY SECRETKEY
```

Replace [[reference/minio-mc-admin/mc-admin-user-add#mc.admin.user.add.ALIAS|`ALIAS`]] with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [[reference/minio-mc-admin/mc-admin-user-add#mc.admin.user.add.ACCESSKEY|`ACCESSKEY`]] with the
access key for the user. MinIO allows retrieving the access key after
user creation through the [[reference/minio-mc-admin/mc-admin-user-info#command-mc.admin.user.info|`mc admin user info`]] command.

Replace [[reference/minio-mc-admin/mc-admin-user-add#mc.admin.user.add.SECRETKEY|`SECRETKEY`]] with the
secret key for the user. MinIO does not provide any method for retrieving
the secret key once set.

Specify a unique, random, and long string for both the `ACCESSKEY` and
`SECRETKEY`. Your organization may have specific internal or regulatory
requirements around generating values for use with access or secret keys.

After creating the user, use [[reference/minio-mc-admin/mc-admin-policy-attach#command-mc.admin.policy.attach|`mc admin policy attach`]] to associate a
[[administration/identity-access-management/policy-based-access-control#minio-policy|MinIO Policy Based Access Control]] to the new user.
The following command assigns the built-in [[administration/identity-access-management/policy-based-access-control#userpolicy.readwrite|`readwrite`]] policy:

```
mc admin policy attach ALIAS readwrite --user=USERNAME
```

Replace `USERNAME` with the `ACCESSKEY` created in the previous step.

### Delete a User

Use the [[reference/minio-mc-admin/mc-admin-user-remove#command-mc.admin.user.rm|`mc admin user rm`]] command to remove a user on a
MinIO deployment:

```
mc admin user rm ALIAS USERNAME
```

Replace [[reference/minio-mc-admin/mc-admin-user-remove#mc.admin.user.rm.ALIAS|`ALIAS`]] with the
[[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] of the MinIO deployment.

Replace [[reference/minio-mc-admin/mc-admin-user-remove#mc.admin.user.rm.USERNAME|`USERNAME`]] with the name of
the user to remove.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).