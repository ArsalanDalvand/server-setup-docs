---
title: "Security and Access — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/console/security-and-access.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/console/security-and-access"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Security and Access — SILO (MinIO Object Storage)"
---
# Security and Access

You can use the MinIO Console to perform several of the identity and access management functions available in MinIO, such as:

Create child [access keys](#minio-console-user-access-keys) that inherit the parent’s permissions.

View, manage, and create access [policies](#minio-console-admin-policies).

Create and manage [user credentials](#minio-console-admin-identity) or groups with the built-in MinIO IDP, connect to one or more OIDC provider, or add an AD/LDAP provider for SSO.

## Access Keys

The Access Keys or Service Accounts section displays all [[administration/identity-access-management/minio-user-management#minio-id-access-keys|Access Keys]] associated to the authenticated user.
The summary list of access keys that already exist for a particular user includes the access key, expiration, status, name, and description.

Access Keys support providing applications authentication credentials which inherit permissions from the “parent” user.

For deployments using an external identity manager such as Active Directory or an OIDC-compatible provider, access keys provide a way for users to create long-lived credentials.

You can select the access key row to view its custom policy, if one exists.

You can create or modify the policy from this screen.
Access key policies cannot exceed the permissions granted to the parent user.

You can create a new access key by selecting Create access key.

The Console auto-generates an access key and password.
You can select the eye  icon on the password field to reveal the value.
You can override these values as needed.

You can set a custom policy for the access key that further restricts the permissions granted to users authenticating with that key.
Select Restrict beyond user policy to open the policy editor and modify as necessary.

Ensure you have saved the access key password to a secure location before selecting Create to create the access key.
You cannot retrieve or reset the password value after creating the access key.

To rotate credentials for an application, create a new access key and delete the old one once the application updates to using the new credentials.

## Policies

The Policies section displays all [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] on the MinIO deployment.
The Policies section allows you to create, modify, or delete policies.

[[administration/identity-access-management/policy-based-access-control#minio-policy|Policies]] define the authorized actions and resources to which an authenticated user has access.
Each policy describes one or more actions a user, group of users, or access key can perform or conditions they must meet.

The policies are JSON formatted text files compatible with Amazon AWS Identity and Access Management policy syntax, structure, and behavior.
Refer to [[administration/identity-access-management/policy-based-access-control#minio-policy|Policy Based Action Control]] for details on managing access in MinIO with policies.

This section or its contents may not be visible if the authenticated user does not have the [[administration/identity-access-management/policy-based-access-control#minio-policy-mc-admin-actions|required administrative permissions]].

Select + Create Policy to create a new MinIO Policy.

Select the policy row to manage the policy details.

The Summary view displays a summary of the policy.

The Users view displays all users assigned to the policy.

The Groups view displays all groups assigned to the policy.

The Raw Policy view displays the raw JSON policy.

Use the Users and Groups views to assign a created policy to users and groups, respectively.

## Identity

The Identity section provides a management interface for [[administration/identity-access-management/minio-user-management#minio-users|MinIO-Managed users]].

The section contains the following subsections.
Some subsections may not be visible if the authenticated user does not have the [[administration/identity-access-management/policy-based-access-control#minio-policy-mc-admin-actions|required administrative permissions]].

### Users

The Users section displays all MinIO-managed  [[administration/identity-access-management/minio-user-management#minio-users|users]] on the deployment.

This section is not visible for deployments using an external identity manager such as Active Directory or an OIDC-compatible provider.

Select Create User to create a new MinIO-managed user.

You can assign [[administration/identity-access-management/minio-group-management#minio-groups|groups]] and [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] to the user during creation.

Select a user’s row to view details for that user.

You can view and modify the user’s assigned [[administration/identity-access-management/minio-group-management#minio-groups|groups]] and [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]].

You can also view and manage any [[administration/identity-access-management/minio-user-management#minio-idp-service-account|Access Keys]] associated to the user.

### Groups

The Groups section displays all [[administration/identity-access-management/minio-group-management#minio-groups|groups]] on the MinIO deployment.

This section is not visible for deployments using an external identity manager such as Active Directory or an OIDC-compatible provider.

Select Create Group to create a new MinIO Group.

You can assign new users to the group during creation.

You can assign policies to the group after creation.

Select the group row to open the details for that group.

You can modify the group membership from the Members view.

You can modify the group’s assigned policies from the Policies view.

Changing a user’s group membership modifies the policies that user inherits. See [[administration/identity-access-management#minio-access-management|Access Management]] for more information.

### OpenID

MinIO supports using an [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid|OpenID Connect (OIDC) compatible IDentity Provider (IDP)]] for external management of user identities.

Examples of OpenID providers include:

Okta

KeyCloak

Dex

Google

Facebook

Configuring an external IDP enables Single-Sign On workflows, where applications authenticate against the external IDP before accessing MinIO.

Use the the screens in this section to view, add, or edit OIDC configurations for the deployment.
MinIO supports any number of active OIDC configurations.

### LDAP

MinIO supports using an [[administration/identity-access-management/ad-ldap-access-management#minio-external-identity-management-ad-ldap|Active Directory or LDAP (AD/LDAP)]] service for external management of user identities.
Configuring an external IDentity Provider (IDP) enables Single-Sign On (SSO) workflows, where applications authenticate against the external IDP before accessing MinIO.

Use the the screens in this section to view, add, or edit an LDAP configuration for the deployment.
MinIO only supports one active LDAP configuration.

MinIO queries the Active Directory / LDAP server to verify the client-specified credentials.
MinIO also performs a group lookup on the AD/LDAP server if configured to do so.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).