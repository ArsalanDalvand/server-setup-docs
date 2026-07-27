---
title: "Configure MinIO for Authentication using Active Directory / LDAP — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/external-iam/configure-ad-ldap-external-identity-management.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/external-iam/configure-ad-ldap-external-identity-management"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Configure MinIO for Authentication using Active Directory / LDAP — SILO (MinIO Object Storage)"
---
# Configure MinIO for Authentication using Active Directory / LDAP

## Overview

MinIO supports configuring a single Active Directory / LDAP Connect for external management of user identities.

The procedure on this page provides instructions for:

Kubernetes

For MinIO Tenants deployed using the [[operations/deployments/kubernetes#minio-kubernetes|MinIO Kubernetes Operator]], this procedure covers:

Configuring a MinIO Tenant to use an external AD/LDAP provider

Accessing the Tenant Console using AD/LDAP Credentials.

Using the MinIO `AssumeRoleWithLDAPIdentity` Security Token Service (STS) API to generate temporary credentials for use by applications.

Baremetal

For MinIO deployments on baremetal infrastructure, this procedure covers:

Configuring a MinIO cluster for an external AD/LDAP provider.

Accessing the MinIO Console using AD/LDAP credentials.

Using the MinIO `AssumeRoleWithLDAPIdentity` Security Token Service (STS) API to generate temporary credentials for use by applications.

This procedure is generic for AD/LDAP services.
See the documentation for the AD/LDAP provider of your choice for specific instructions or procedures on configuration of user identities.

## Prerequisites

### Access to MinIO Cluster

Kubernetes

You must have access to the MinIO Operator Console web UI.
You can either expose the MinIO Operator Console service using your preferred Kubernetes routing component, or use temporary port forwarding to expose the Console service port on your local machine.

Baremetal

This procedure uses [[reference/minio-mc#command-mc|`mc`]] for performing operations on the MinIO cluster.
Install `mc` on a machine with network access to the cluster.
See the `mc` [[reference/minio-mc#mc-install|Installation Quickstart]] for instructions on downloading and installing `mc`.

This procedure assumes a configured [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] for the MinIO cluster.

### Active Directory / LDAP Compatible IDentity Provider

This procedure assumes an existing Active Directory or LDAP service.
Instructions on configuring AD/LDAP are out of scope for this procedure.

Kubernetes

For AD/LDAP deployments within the same Kubernetes cluster as the MinIO Tenant, you can use Kubernetes service names to allow the MinIO Tenant to establish connectivity to the AD/LDAP service.

For AD/LDAP deployments external to the Kubernetes cluster, you must ensure the cluster supports routing communications between Kubernetes services and pods and the external network.
This may require configuration or deployment of additional Kubernetes network components and/or enabling access to the public internet.

Baremetal

The MinIO deployment must have bidirectional network connectivity to the target AD / LDAP service.

MinIO requires a read-only access keys with which it [[operations/external-iam#minio-external-identity-management-ad-ldap-lookup-bind|binds]] to perform authenticated user and group queries.
Ensure each AD/LDAP user and group intended for use with MinIO has a corresponding [[operations/external-iam#minio-external-identity-management-ad-ldap-access-control|policy]] on the MinIO deployment.
An AD/LDAP user with no assigned policy and with membership in groups with no assigned policy has no permission to access any action or resource on the MinIO cluster.

## Configure MinIO with Active Directory or LDAP External Identity Management

Set the Active Directory / LDAP Configuration Settings

Configure the AD/LDAP provider using one of the following:

MinIO Client

Environment variables

All methods require starting/restarting the MinIO deployment to apply changes.

The following tabs provide a quick reference for the available configuration methods:

MinIO Client

MinIO supports specifying the AD/LDAP provider settings using [[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`mc idp ldap`]] commands.

For distributed deployments, the [[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`mc idp ldap`]] command applies the configuration to all nodes in the deployment.

The following example code sets all configuration settings related to configuring an AD/LDAP provider for external identity management.

The minimum required settings are:

[[reference/minio-server/settings/iam/ldap#mc-conf.identity_ldap.server_addr|`server_addr`]]

[[reference/minio-server/settings/iam/ldap#mc-conf.identity_ldap.lookup_bind_dn|`lookup_bind_dn`]]

[[reference/minio-server/settings/iam/ldap#mc-conf.identity_ldap.lookup_bind_password|`lookup_bind_password`]]

[[reference/minio-server/settings/iam/ldap#mc-conf.identity_ldap.user_dn_search_base_dn|`user_dn_search_base_dn`]]

[[reference/minio-server/settings/iam/ldap#mc-conf.identity_ldap.user_dn_search_filter|`user_dn_search_filter`]]

```
mc idp ldap add ALIAS                                                  \
  server_addr="ldaps.example.net:636"                                  \
  lookup_bind_dn="CN=xxxxx,OU=xxxxx,OU=xxxxx,DC=example,DC=net"        \
  lookup_bind_password="xxxxxxxx"                                      \
  user_dn_search_base_dn="DC=example,DC=net"                           \
  user_dn_search_filter="(&(objectCategory=user)(sAMAccountName=%s))"  \
  group_search_filter= "(&(objectClass=group)(member=%d))"             \
  group_search_base_dn="ou=MinIO Users,dc=example,dc=net"              \
  tls_skip_verify="off"                                                \
  server_insecure=off                                                  \
  server_starttls="off"                                                \
  srv_record_name=""                                                   \
  comment="Test LDAP server"
```

For Kubernetes deployments, ensure the ALIAS corresponds to the externally accessible hostname for the MinIO Tenant.

For more complete documentation on these settings, see [[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`mc idp ldap`]].

[[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`mc idp ldap`]] recommended

[[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`mc idp ldap`]] offers additional features and improved validation over [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] runtime configuration settings.
[[reference/minio-mc/mc-idp-ldap#command-mc.idp.ldap|`mc idp ldap`]] supports the same settings as [[reference/minio-mc-admin/mc-admin-config#command-mc.admin.config|`mc admin config`]] and the [[reference/minio-server/settings/iam/ldap#mc-conf.identity_ldap|`identity_ldap`]] configuration key.

The [[reference/minio-server/settings/iam/ldap#mc-conf.identity_ldap|`identity_ldap`]] configuration key remains available for existing scripts and tools.

Environment Variables

MinIO supports specifying the AD/LDAP provider settings using [[reference/minio-server/settings/iam/ldap#minio-server-envvar-external-identity-management-ad-ldap|environment variables]].

The [[reference/minio-server/minio-server#command-minio.server|`minio server`]] process applies the specified settings on its next startup.
For distributed deployments, specify these settings across all nodes in the deployment using the same values.
Any differences in server configurations between nodes will result in startup or configuration failures.

The following example code sets all environment variables related to configuring an AD/LDAP provider for external identity management. The minimum required variable are:

[[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_SERVER_ADDR|`MINIO_IDENTITY_LDAP_SERVER_ADDR`]]

[[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_LOOKUP_BIND_DN|`MINIO_IDENTITY_LDAP_LOOKUP_BIND_DN`]]

[[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_LOOKUP_BIND_PASSWORD|`MINIO_IDENTITY_LDAP_LOOKUP_BIND_PASSWORD`]]

[[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_USER_DN_SEARCH_BASE_DN|`MINIO_IDENTITY_LDAP_USER_DN_SEARCH_BASE_DN`]]

[[reference/minio-server/settings/iam/ldap#envvar.MINIO_IDENTITY_LDAP_USER_DN_SEARCH_FILTER|`MINIO_IDENTITY_LDAP_USER_DN_SEARCH_FILTER`]]

```
export MINIO_IDENTITY_LDAP_SERVER_ADDR="ldaps.example.net:636"
export MINIO_IDENTITY_LDAP_LOOKUP_BIND_DN="CN=xxxxx,OU=xxxxx,OU=xxxxx,DC=example,DC=net"
export MINIO_IDENTITY_LDAP_USER_DN_SEARCH_BASE_DN="dc=example,dc=net"
export MINIO_IDENTITY_LDAP_USER_DN_SEARCH_FILTER="(&(objectCategory=user)(sAMAccountName=%s))"
export MINIO_IDENTITY_LDAP_LOOKUP_BIND_PASSWORD="xxxxxxxxx"
export MINIO_IDENTITY_LDAP_GROUP_SEARCH_FILTER="(&(objectClass=group)(member=%d))"
export MINIO_IDENTITY_LDAP_GROUP_SEARCH_BASE_DN="ou=MinIO Users,dc=example,dc=net"
export MINIO_IDENTITY_LDAP_TLS_SKIP_VERIFY="off"
export MINIO_IDENTITY_LDAP_SERVER_INSECURE="off"
export MINIO_IDENTITY_LDAP_SERVER_STARTTLS="off"
export MINIO_IDENTITY_LDAP_SRV_RECORD_NAME=""
export MINIO_IDENTITY_LDAP_COMMENT="LDAP test server"
```

For complete documentation on these variables, see [[reference/minio-server/settings/iam/ldap#minio-server-envvar-external-identity-management-ad-ldap|Active Directory / LDAP Settings]].

Restart the MinIO Deployment

You must restart the MinIO deployment to apply the configuration changes.

If you configured AD/LDAP from the MinIO Console, no additional action is required.
The MinIO Console automatically restarts the deployment after saving the new AD/LDAP configuration.

For MinIO Client and environment variable configuration, use the [[reference/minio-mc-admin/mc-admin-service#mc.admin.service.restart|`mc admin service restart`]] command to restart the deployment:

```
mc admin service restart ALIAS
```

Replace `ALIAS` with the [[reference/minio-mc/mc-alias-set#alias|alias]] of the deployment to restart.

Use the MinIO Console to Log In with AD/LDAP Credentials

The MinIO Console supports the full workflow of authenticating to the AD/LDAP provider, generating temporary credentials using the MinIO [[developers/security-token-service/AssumeRoleWithLDAPIdentity#minio-sts-assumerolewithldapidentity|AssumeRoleWithLDAPIdentity]] Security Token Service (STS) endpoint, and logging the user into the MinIO deployment.

You can access the Console by opening the root URL for the MinIO cluster. For example, `https://minio.example.net:9000`.

Once logged in, you can perform any action for which the authenticated user is [[operations/external-iam#minio-external-identity-management-ad-ldap-access-control|authorized]].

You can also create [[administration/identity-access-management/minio-user-management#minio-idp-service-account|access keys]] for supporting applications which must perform operations on MinIO.
Access Keys are long-lived credentials which inherit their privileges from the parent user.
The parent user can further restrict those privileges while creating the service account.

Generate S3-Compatible Temporary Credentials using AD/LDAP Credentials

MinIO requires clients to authenticate using [AWS Signature Version 4 protocol](https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-authenticating-requests.html) with support for the deprecated Signature Version 2 protocol.
Specifically, clients must present a valid access key and secret key to access any S3 or MinIO administrative API, such as `PUT`, `GET`, and `DELETE` operations.

Applications can generate temporary access credentials as-needed using the [[developers/security-token-service/AssumeRoleWithLDAPIdentity#minio-sts-assumerolewithldapidentity|AssumeRoleWithLDAPIdentity]] Security Token Service (STS) API endpoint and AD/LDAP user credentials.
MinIO provides an example Go application [ldap.go](https://github.com/minio/minio/blob/master/docs/sts/ldap.go) that manages this workflow.

```
POST https://minio.example.net?Action=AssumeRoleWithLDAPIdentity
&LDAPUsername=USERNAME
&LDAPPassword=PASSWORD
&Version=2011-06-15
&Policy={}
```

Replace the `LDAPUsername` with the username of the AD/LDAP user.

Replace the `LDAPPassword` with the password of the AD/LDAP user.

Replace the `Policy` with an inline URL-encoded JSON [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]] that further restricts the permissions associated to the temporary credentials.

Omit to use the  [[operations/external-iam#minio-external-identity-management-ad-ldap-access-control|policy whose name matches]] the Distinguished Name (DN) of the AD/LDAP user.

The API response consists of an XML document containing the access key, secret key, session token, and expiration date.
Applications can use the access key and secret key to access and perform operations on MinIO.

See the [[developers/security-token-service/AssumeRoleWithLDAPIdentity#minio-sts-assumerolewithldapidentity|AssumeRoleWithLDAPIdentity]] for reference documentation.

## Disable a Configured Active Directory / LDAP Connection

New in version RELEASE.2023-03-20T20-16-18Z.

You can enable and disable the configured AD/LDAP connection as needed.

Use [[reference/minio-mc/mc-idp-ldap-disable#command-mc.idp.ldap.disable|`mc idp ldap disable`]] to deactivate a configured connection.
Use [[reference/minio-mc/mc-idp-ldap-enable#command-mc.idp.ldap.enable|`mc idp ldap enable`]] to activate a previously configured connection.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).