---
title: "Active Directory / LDAP Access Management — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/identity-access-management/ad-ldap-access-management.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/identity-access-management/ad-ldap-access-management"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Active Directory / LDAP Access Management — SILO (MinIO Object Storage)"
---
# Active Directory / LDAP Access Management

MinIO supports configuring a single Active Directory or LDAP (AD/LDAP) service for external management of user identities.
Enabling AD/LDAP external identity management disables the [[administration/identity-access-management/minio-identity-management#minio-internal-idp|MinIO internal IDP]].

For identities managed by the external AD/LDAP provider, MinIO uses the user’s Distinguished Name and attempts to map it against an existing [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]].

If the AD/LDAP configuration includes the necessary settings to query the user’s AD/LDAP group membership, MinIO also uses those group Distinguished Names and attempts to map each against an existing [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]].

MinIO by default denies access to all actions or resources not explicitly allowed by a user’s assigned or inherited [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]].
Users managed by an AD/LDAP provider must specify the necessary policies as part of the user profile data.
If no policies match either the user DN or group DNs, MinIO blocks all access to actions and resources on the deployment.

The specific AD/LDAP queries MinIO issues to authenticate the user and retrieve it’s group membership are configured as part of [[operations/external-iam#minio-external-iam-ad-ldap|deploying the cluster with Active Directory / LDAP identity management]].
This page covers creation of MinIO policies to match the possible returned Distinguished Names.

## Authentication and Authorization Flow

The login flow for an application using Active Directory / LDAP
credentials is as follows:

Specify the AD/LDAP credentials to the MinIO Security Token Service (STS)
[[developers/security-token-service/AssumeRoleWithLDAPIdentity#minio-sts-assumerolewithldapidentity|AssumeRoleWithLDAPIdentity]] API endpoint.

MinIO verifies the provided credentials against the AD/LDAP server.

MinIO checks for any [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]] whose name matches the
user Distinguished Name (DN) and assigns that policy to the authenticated
user.

If configured to perform group queries, MinIO also queries for a list of
AD/LDAP groups in which the user has membership. MinIO checks for any policy
whose name matches a returned group DN and assigns that
policy to the authenticated user.

MinIO returns temporary credentials in the STS API response in the form of an
access key, secret key, and session token. The credentials have permissions
matching those policies whose name matches either the authenticated user DN
or a group DN.

MinIO provides an example Go application
[ldap.go](https://github.com/minio/minio/blob/master/docs/sts/ldap.go) that handles the
full login flow.

AD/LDAP users can alternatively create [[administration/identity-access-management/minio-user-management#minio-idp-service-account|access keys]] associated to their AD/LDAP user Distinguished Name.
Access Keys are long-lived credentials which inherit their privileges from the parent user.
The parent user can further restrict those privileges while creating the access keys.
Use either of the following methods to create a new access key:

Use the [[reference/minio-mc-admin/mc-admin-user-svcacct-add#command-mc.admin.user.svcacct.add|`mc admin user svcacct add`]] command to create the access keys.
Specify the user Distinguished Name as the username to which to associate the access keys.

## Mapping Policies to User DN

The following commands use [[reference/minio-mc/mc-idp-ldap-policy-attach#command-mc.idp.ldap.policy.attach|`mc idp ldap policy attach`]] to associate an existing MinIO [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]] to an AD/LDAP User DN.

```
mc idp ldap policy attach myminio consoleAdmin \
  --user='cn=sisko,cn=users,dc=example,dc=com'

mc idp ldap policy attach myminio readwrite,diagnostics \
  --user='cn=dax,cn=users,dc=example,dc=com'
```

MinIO would assign an authenticated user with DN matching
`cn=sisko,cn=users,dc=example,dc=com` the [[administration/identity-access-management/policy-based-access-control#userpolicy.consoleAdmin|`consoleAdmin`]]
policy, granting complete access to the MinIO server.

MinIO would assign an authenticated user with DN matching
`cn=dax,cn=users,dc=example,dc=com` both the [[administration/identity-access-management/policy-based-access-control#userpolicy.readwrite|`readwrite`]] and
[[administration/identity-access-management/policy-based-access-control#userpolicy.diagnostics|`diagnostics`]] policies, granting general read/write access to the
MinIO server and access to diagnostic administrative operations.

MinIO would assign no policies to an authenticated user with DN matching
`cn=quark,cn=users,dc=example,dc=com` and deny all access to API operations.

## Mapping Policies to Group DN

The following commands use [[reference/minio-mc/mc-idp-ldap-policy-attach#command-mc.idp.ldap.policy.attach|`mc idp ldap policy attach`]] to associate an existing MinIO [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]] to an AD/LDAP Group DN.

```
mc idp ldap policy attach myminio consoleAdmin \
  --group='cn=ops,cn=groups,dc=example,dc=com'

mc idp ldap policy attach myminio diagnostics \
  --group='cn=engineering,cn=groups,dc=example,dc=com'
```

MinIO would assign any authenticating user with membership in the
`cn=ops,cn=groups,dc=example,dc=com` AD/LDAP group the
[[administration/identity-access-management/policy-based-access-control#userpolicy.consoleAdmin|`consoleAdmin`]] policy, granting complete access to the MinIO
server.

MinIO would assign any authenticating user with membership in the
`cn=engineering,cn=groups,dc=example,dc=com` AD/LDAP group the
[[administration/identity-access-management/policy-based-access-control#userpolicy.diagnostics|`diagnostics`]] policy, granting access to diagnostic administrative
operations.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).