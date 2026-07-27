---
title: "Configure MinIO for Authentication using Keycloak — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/operations/external-iam/configure-keycloak-identity-management.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operations/external-iam/configure-keycloak-identity-management"
category: "operations"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Configure MinIO for Authentication using Keycloak — SILO (MinIO Object Storage)"
---
# Configure MinIO for Authentication using Keycloak

## Overview

This procedure configures MinIO to use [Keycloak](https://www.keycloak.org/) as an external IDentity Provider (IDP) for authentication of users via the OpenID Connect (OIDC) protocol.

This page has procedures for configuring OIDC for MinIO deployments in Kubernetes and Baremetal infrastructures.

Select the tab corresponding to your infrastructure to switch between instruction sets.

Kubernetes

For MinIO Tenants deployed using the [[operations/deployments/kubernetes#minio-kubernetes|MinIO Kubernetes Operator]], this procedure covers:

Configure Keycloak for use with MinIO authentication and authorization

Configure a new or existing MinIO Tenant to use Keycloak as the OIDC provider

Create policies to control access of Keycloak-authenticated users

Log into the MinIO Tenant Console using SSO and a Keycloak-managed identity

Generate temporary S3 access credentials using the `AssumeRoleWithWebIdentity` Security Token Service (STS) API

Baremetal

For MinIO deployments on baremetal infrastructure, this procedure covers:

Configure Keycloak for use with MinIO authentication and authorization

Configure a new or existing MinIO cluster to use Keycloak as the OIDC provider

Create policies to control access of Keycloak-authenticated users

Log into the MinIO Console using SSO and a Keycloak-managed identity

Generate temporary S3 access credentials using the `AssumeRoleWithWebIdentity` Security Token Service (STS) API

This procedure was written and tested against Keycloak `21.0.0`.
The provided instructions may work against other Keycloak versions.
This procedure assumes you have prior experience with Keycloak and have reviewed [their documentation](https://www.keycloak.org/documentation) for guidance and best practices in deploying, configuring, and managing the service.

## Prerequisites

### Keycloak Deployment and Realm Configuration

This procedure assumes an existing Keycloak deployment to which you have administrative access.
Specifically, you must have permission to create and configure Realms, Clients, Client Scopes, Realm Roles, Users, and Groups on the Keycloak deployment.

Kubernetes

For Keycloak deployments within the same Kubernetes cluster as the MinIO Tenant, this procedure assumes bidirectional access between the Keycloak and MinIO pods/services.
For Keycloak deployments external to the Kubernetes cluster, this procedure assumes an existing Ingress, Load Balancer, or similar Kubernetes network control component that manages network access to and from the MinIO Tenant.

Baremetal

The MinIO deployment must have bidirectional access to the target OIDC service.

Ensure each user identity intended for use with MinIO has the appropriate [[administration/identity-access-management/oidc-access-management#minio-external-identity-management-openid-access-control|claim]] configured such that MinIO can associate a [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]] to the authenticated user.
An OpenID user with no assigned policy has no permission to access any action or resource on the MinIO cluster.

### Access to MinIO Cluster

Kubernetes

You must have access to the MinIO Operator Console web UI.
You can either expose the MinIO Operator Console service using your preferred Kubernetes routing component, or use temporary port forwarding to expose the Console service port on your local machine.

Baremetal

This procedure uses [[reference/minio-mc#command-mc|`mc`]] for performing operations on the MinIO cluster.
Install `mc` on a machine with network access to the cluster.
See the `mc` [[reference/minio-mc#mc-install|Installation Quickstart]] for instructions on downloading and installing `mc`.

This procedure assumes a configured [[reference/minio-mc/mc-alias#command-mc.alias|`alias`]] for the MinIO cluster.

## Configure MinIO for Keycloak Identity Management

Kubernetes

Configure or Create a Client for Accessing Keycloak

Authenticate to the Keycloak Administrative Console and navigate to Clients.

Select Create client and follow the instructions to create a new Keycloak client for MinIO.
Fill in the specified inputs as follows:

Client ID

Set to a unique identifier for MinIO (`minio`)

Client type

Set to `OpenID Connect`

Always display in console

Toggle to `On`

Client authentication

Toggle to `On`

Authentication flow

Toggle on `Standard flow`

(Optional) Authentication flow

Toggle on `Direct access grants` (API testing)

Keycloak deploys the client with a default set of configuration values.
Modify these values as necessary for your Keycloak setup and desired behavior.
The following table provides a baseline of settings and values to configure:

Root URL

Set to `${authBaseUrl}`

Home URL

Set to the Realm you want MinIO to use (`/realms/master/account/`)

Valid Redirect URI

Set to `*`

Keys -> Use JWKS URL

Toggle to `On`

Advanced -> Advanced Settings -> Access Token Lifespan

Set to `1 Hour`.

Create Client Scope for MinIO Client

Client scopes allow Keycloak to map user attributes as part of the JSON Web Token (JWT) returned in authentication requests.
This allows MinIO to reference those attributes when assigning policies to the user.
This step creates the necessary client scope to support MinIO authorization after successful Keycloak authentication.

Navigate to the Client scopes view and create a new client scope for MinIO authorization:

Name

Set to any recognizable name for the policy (`minio-authorization`)

Include in token scope

Toggle to `On`

Once created, select the scope from the list and navigate to Mappers.

Select Configure a new mapper to create a new mapping:

User Attribute

Select the Mapper Type

Name

Set to any recognizable name for the mapping (`minio-policy-mapper`)

User Attribute

Set to `policy`

Token Claim Name

Set to `policy`

Add to ID token

Set to `On`

Claim JSON Type

Set to `String`

Multivalued

Set to `On`

This allows setting multiple `policy` values in the single claim.

Aggregate attribute values

Set to `On`

This allows users to inherit any `policy` set in their Groups

Once created, assign the Client Scope to the MinIO client.

Navigate to Clients and select the MinIO client.

Select Client scopes, then select Add client scope.

Select the previously created scope and set the Assigned type to `default`.

Apply the Necessary Attribute to Keycloak Users/Groups

You must assign an attribute named `policy` to the Keycloak Users or Groups.
Set the value to any [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]] on the MinIO deployment.

For Users, navigate to Users and select or create the User:

Credentials

Set the user password to a permanent value if not already set

Attributes

Create a new attribute with key `policy` and value of any [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]] (`consoleAdmin`)

For Groups, navigate to Groups and select or create the Group:

Attributes

Create a new attribute with key `policy` and value of any [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]] (`consoleAdmin`)

You can assign users to groups such that they inherit the specified `policy` attribute.
If you set the Mapper settings to enable Aggregate attribute values, Keycloak includes the aggregated array of policies as part of the authenticated user’s JWT token.
MinIO can use this list of policies when authorizing the user.

You can test the configured policies of a user by using the Keycloak API:

```
curl -d "client_id=minio" \
     -d "client_secret=secretvalue" \
     -d "grant_type=password" \
     -d "username=minio-user-1" \
     -d "password=minio-user-1-password" \
     http://keycloak-service.keycloak-namespace.svc.cluster-domain.example/realms/REALM/protocol/openid-connect/token
```

If successful, the `access_token` contains the JWT necessary to use the MinIO [[developers/security-token-service/AssumeRoleWithWebIdentity#minio-sts-assumerolewithwebidentity|AssumeRoleWithWebIdentity]] STS API and generate S3 credentials.

You can use a JWT decoder to review the payload and ensure it contains the `policy` key with one or more MinIO policies listed.

Configure MinIO for Keycloak Authentication

You can use the [[reference/minio-mc/mc-idp-openid#mc.idp.openid.add|`mc idp openid add`]] command to create a new configuration for the Keycloak service.
The command takes all supported [[reference/minio-server/settings/iam/openid#minio-open-id-config-settings|OpenID Configuration Settings]]:

```
mc idp openid add ALIAS PRIMARY_IAM \
   client_id=MINIO_CLIENT \
   client_secret=MINIO_CLIENT_SECRET \
   config_url="https://keycloak-service.keycloak-namespace.svc.cluster-domain.example/realms/REALM/.well-known/openid-configuration" \
   display_name="SSO_IDENTIFIER"
   scopes="openid,email,preferred_username" \
   redirect_uri_dynamic="on"
```

`PRIMARY_IAM`

Set to a unique identifier for the Keycloak service, such as `keycloak_primary`

`MINIO_CLIENT`
`MINIO_CLIENT_SECRET`

Set to the Keycloak client ID and secret configured in Step 1

`config_url`

Set to the address of the Keycloak OpenID configuration document (keycloak-url.example.net:8080)

`display_name`

Set to a user-facing name the MinIO Console displays as part of the Single-Sign On (SSO) workflow for the configured Keycloak service

`scopes`

Set to a list of OpenID scopes you want to include in the JWT, such as `preferred_username` or `email`

`redirect_uri_dynamic`

Set to `on`

Substitutes the MinIO Console address used by the client as part of the Keycloak redirect URI.
Keycloak returns authenticated users to the Console using the provided URI.

For MinIO Console deployments behind a reverse proxy, load balancer, or similar network control plane, you can instead use the [[reference/minio-server/settings/console#envvar.MINIO_BROWSER_REDIRECT_URL|`MINIO_BROWSER_REDIRECT_URL`]] variable to set the redirect address for Keycloak to use.

Restart the MinIO deployment for the changes to apply.

Check the MinIO logs and verify that startup succeeded with no errors related to the OIDC configuration.

Generate Application Credentials using the Security Token Service (STS)

Applications using an S3-compatible SDK must specify credentials in the form of an access key and secret key.
The MinIO [[developers/security-token-service/AssumeRoleWithWebIdentity#minio-sts-assumerolewithwebidentity|AssumeRoleWithWebIdentity]] API returns the necessary temporary credentials, including a required session token, using a JWT returned by Keycloak after authentication.

You can test this workflow using the following sequence of HTTP calls and the `curl` utility:

Authenticate as a Keycloak user and retrieve the JWT token

```
curl -X POST "https://keycloak-service.keycloak-namespace.svc.cluster-domain.example/realms/REALM/protocol/openid-connect/token" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -d "username=USER" \
     -d "password=PASSWORD" \
     -d "grant_type=password" \
     -d "client_id=CLIENT" \
     -d "client_secret=SECRET"
```

Replace the `USER` and `PASSWORD` with the credentials of a Keycloak user on the `REALM`.

Replace the `CLIENT` and `SECRET` with the client ID and secret for the MinIO-specific Keycloak client on the `REALM`

You can process the results using `jq` or a similar JSON-formatting utility.
Extract the `access_token` field to retrieve the necessary access token.
Pay attention to the `expires_in` field to note the number of seconds before the token expires.

Generate MinIO Credentials using the `AssumeRoleWithWebIdentity` API

```
curl -X POST "https://minio.minio-tenant.svc.cluster-domain.example" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -d "Action=AssumeRoleWithWebIdentity" \
     -d "Version=2011-06-15" \
     -d "DurationSeconds=86000" \
     -d "WebIdentityToken=TOKEN"
```

Replace the `TOKEN` with the `access_token` value returned by Keycloak.

The API returns an XML document on success containing the following keys:

`Credentials.AccessKeyId` - the Access Key for the Keycloak User

`Credentials.SecretAccessKey` - the Secret Key for the Keycloak User

`Credentials.SessionToken` - the Session Token for the Keycloak User

`Credentials.Expiration` - the Expiration Date for the generated credentials

Test the Credentials

Use your preferred S3-compatible SDK to connect to MinIO using the generated credentials.

For example, the following Python code using the MinIO [[developers/python/minio-py#minio-python-quickstart|Python SDK]] connects to the MinIO deployment and returns a list of buckets:

```
from minio import Minio

client = MinIO(
   "minio.minio-tenant.svc.cluster-domain.example",
   access_key = "ACCESS_KEY",
   secret_key = "SECRET_KEY",
   session_token = "SESSION_TOKEN"
   secure = True
)

client.list_buckets()
```

Next Steps

Applications should implement the [[developers/security-token-service/AssumeRoleWithWebIdentity#minio-sts-assumerolewithwebidentity|STS AssumeRoleWithWebIdentity]] flow using their [[developers/minio-drivers#minio-drivers|SDK]] of choice.
When STS credentials expire, applications should have logic in place to regenerate the JWT token, STS token, and MinIO credentials before retrying and continuing operations.

Alternatively, users can generate [[administration/identity-access-management/minio-user-management#minio-id-access-keys|access keys]] through the MinIO Console for the purpose of creating long-lived API-key like access using their Keycloak credentials.

Baremetal

Configure or Create a Client for Accessing Keycloak

Authenticate to the Keycloak Administrative Console and navigate to Clients.

Select Create client and follow the instructions to create a new Keycloak client for MinIO.
Fill in the specified inputs as follows:

Client ID

Set to a unique identifier for MinIO (`minio`)

Client type

Set to `OpenID Connect`

Always display in console

Toggle to `On`

Client authentication

Toggle to `On`

Authentication flow

Toggle on `Standard flow`

(Optional) Authentication flow

Toggle on `Direct access grants` (API testing)

Keycloak deploys the client with a default set of configuration values.
Modify these values as necessary for your Keycloak setup and desired behavior.
The following table provides a baseline of settings and values to configure:

Root URL

Set to `${authBaseUrl}`

Home URL

Set to the Realm you want MinIO to use (`/realms/master/account/`)

Valid Redirect URI

Set to `*`

Keys -> Use JWKS URL

Toggle to `On`

Advanced -> Advanced Settings -> Access Token Lifespan

Set to `1 Hour`.

Create Client Scope for MinIO Client

Client scopes allow Keycloak to map user attributes as part of the JSON Web Token (JWT) returned in authentication requests.
This allows MinIO to reference those attributes when assigning policies to the user.
This step creates the necessary client scope to support MinIO authorization after successful Keycloak authentication.

Navigate to the Client scopes view and create a new client scope for MinIO authorization:

Name

Set to any recognizable name for the policy (`minio-authorization`)

Include in token scope

Toggle to `On`

Once created, select the scope from the list and navigate to Mappers.

Select Configure a new mapper to create a new mapping:

User Attribute

Select the Mapper Type

Name

Set to any recognizable name for the mapping (`minio-policy-mapper`)

User Attribute

Set to `policy`

Token Claim Name

Set to `policy`

Add to ID token

Set to `On`

Claim JSON Type

Set to `String`

Multivalued

Set to `On`

This allows setting multiple `policy` values in the single claim.

Aggregate attribute values

Set to `On`

This allows users to inherit any `policy` set in their Groups

Once created, assign the Client Scope to the MinIO client.

Navigate to Clients and select the MinIO client.

Select Client scopes, then select Add client scope.

Select the previously created scope and set the Assigned type to `default`.

Apply the Necessary Attribute to Keycloak Users/Groups

You must assign an attribute named `policy` to the Keycloak Users or Groups.
Set the value to any [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]] on the MinIO deployment.

For Users, navigate to Users and select or create the User:

Credentials

Set the user password to a permanent value if not already set

Attributes

Create a new attribute with key `policy` and value of any [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]] (`consoleAdmin`)

For Groups, navigate to Groups and select or create the Group:

Attributes

Create a new attribute with key `policy` and value of any [[administration/identity-access-management/policy-based-access-control#minio-policy|policy]] (`consoleAdmin`)

You can assign users to groups such that they inherit the specified `policy` attribute.
If you set the Mapper settings to enable Aggregate attribute values, Keycloak includes the aggregated array of policies as part of the authenticated user’s JWT token.
MinIO can use this list of policies when authorizing the user.

You can test the configured policies of a user by using the Keycloak API:

```
curl -d "client_id=minio" \
     -d "client_secret=secretvalue" \
     -d "grant_type=password" \
     -d "username=minio-user-1" \
     -d "password=minio-user-1-password" \
     http://keycloak-url.example.net:8080/realms/REALM/protocol/openid-connect/token
```

If successful, the `access_token` contains the JWT necessary to use the MinIO [[developers/security-token-service/AssumeRoleWithWebIdentity#minio-sts-assumerolewithwebidentity|AssumeRoleWithWebIdentity]] STS API and generate S3 credentials.

You can use a JWT decoder to review the payload and ensure it contains the `policy` key with one or more MinIO policies listed.

Configure MinIO for Keycloak Authentication

MinIO supports multiple methods for configuring Keycloak authentication:

Using a terminal/shell and the [[reference/minio-mc/mc-idp-openid#command-mc.idp.openid|`mc idp openid`]] command

Using environment variables set prior to starting MinIO

CLI

You can use the [[reference/minio-mc/mc-idp-openid#mc.idp.openid.add|`mc idp openid add`]] command to create a new configuration for the Keycloak service.
The command takes all supported [[reference/minio-server/settings/iam/openid#minio-open-id-config-settings|OpenID Configuration Settings]]:

```
mc idp openid add ALIAS PRIMARY_IAM \
   client_id=MINIO_CLIENT \
   client_secret=MINIO_CLIENT_SECRET \
   config_url="https://keycloak-url.example.net:8080/realms/REALM/.well-known/openid-configuration" \
   display_name="SSO_IDENTIFIER"
   scopes="openid,email,preferred_username" \
   redirect_uri_dynamic="on"
```

`PRIMARY_IAM`

Set to a unique identifier for the Keycloak service, such as `keycloak_primary`

`MINIO_CLIENT`
`MINIO_CLIENT_SECRET`

Set to the Keycloak client ID and secret configured in Step 1

`config_url`

Set to the address of the Keycloak OpenID configuration document (keycloak-url.example.net:8080)

`display_name`

Set to a user-facing name the MinIO Console displays as part of the Single-Sign On (SSO) workflow for the configured Keycloak service

`scopes`

Set to a list of OpenID scopes you want to include in the JWT, such as `preferred_username` or `email`

`redirect_uri_dynamic`

Set to `on`

Substitutes the MinIO Console address used by the client as part of the Keycloak redirect URI.
Keycloak returns authenticated users to the Console using the provided URI.

For MinIO Console deployments behind a reverse proxy, load balancer, or similar network control plane, you can instead use the [[reference/minio-server/settings/console#envvar.MINIO_BROWSER_REDIRECT_URL|`MINIO_BROWSER_REDIRECT_URL`]] variable to set the redirect address for Keycloak to use.

Environment Variables

Set the following [[reference/minio-server/settings/iam/openid#minio-server-envvar-external-identity-management-openid|environment variables]] prior to starting the container using the `-e ENVVAR=VALUE` flag.

The following example code sets the minimum required environment variables related to configuring Keycloak as an external identity management provider.

```
MINIO_IDENTITY_OPENID_CONFIG_URL_PRIMARY_IAM="https://keycloak-url.example.net:8080/realms/REALM/.well-known/openid-configuration"
MINIO_IDENTITY_OPENID_CLIENT_ID_PRIMARY_IAM="MINIO_CLIENT"
MINIO_IDENTITY_OPENID_CLIENT_SECRET_PRIMARY_IAM="MINIO_CLIENT_SECRET"
MINIO_IDENTITY_OPENID_DISPLAY_NAME_PRIMARY_IAM="SSO_IDENTIFIER"
MINIO_IDENTITY_OPENID_SCOPES_PRIMARY_IAM="openid,email,preferred_username"
MINIO_IDENTITY_OPENID_REDIRECT_URI_DYNAMIC_PRIMARY_IAM="on"
```

`_PRIMARY_IAM`

Replace the suffix `_PRIMARY_IAM` with a unique identifier for this Keycloak configuration.
For example, `MINIO_IDENTITY_OPENID_CONFIG_URL_KEYCLOAK_PRIMARY`.

You can omit the suffix if you intend to only configure a single OIDC provider for the deployment.

[[reference/minio-server/settings/iam/openid#envvar.MINIO_IDENTITY_OPENID_CONFIG_URL|`CONFIG_URL`]]

Specify the address of the Keycloak OpenID configuration document (keycloak-url.example.net:8080)

Ensure the `REALM` matches the Keycloak realm you want to use for authenticating users to MinIO

[[reference/minio-server/settings/iam/openid#envvar.MINIO_IDENTITY_OPENID_CLIENT_ID|`CLIENT_ID`]]
[[reference/minio-server/settings/iam/openid#envvar.MINIO_IDENTITY_OPENID_CLIENT_SECRET|`CLIENT_SECRET`]]

Specify the Keycloak client ID and secret configured in Step 1

[[reference/minio-server/settings/iam/openid#envvar.MINIO_IDENTITY_OPENID_DISPLAY_NAME|`DISPLAY_NAME`]]

Specify the user-facing name the MinIO Console displays as part of the Single-Sign On (SSO) workflow for the configured Keycloak service

[[reference/minio-server/settings/iam/openid#envvar.MINIO_IDENTITY_OPENID_SCOPES|`OPENID_SCOPES`]]

Specify the OpenID scopes you want to include in the JWT, such as `preferred_username` or `email`

[[reference/minio-server/settings/iam/openid#envvar.MINIO_IDENTITY_OPENID_REDIRECT_URI_DYNAMIC|`REDIRECT_URI_DYNAMIC`]]

Set to `on`

Substitutes the MinIO Console address used by the client as part of the Keycloak redirect URI.
Keycloak returns authenticated users to the Console using the provided URI.

For MinIO Console deployments behind a reverse proxy, load balancer, or similar network control plane, you can instead use the [[reference/minio-server/settings/console#envvar.MINIO_BROWSER_REDIRECT_URL|`MINIO_BROWSER_REDIRECT_URL`]] variable to set the redirect address for Keycloak to use.

For complete documentation on these variables, see [[reference/minio-server/settings/iam/openid#minio-server-envvar-external-identity-management-openid|OpenID Identity Management Settings]]

Restart the MinIO deployment for the changes to apply.

Check the MinIO logs and verify that startup succeeded with no errors related to the OIDC configuration.

If you attempt to log in with the Console, you should now see an (SSO) button using the configured Display Name.

Specify a configured user and attempt to log in.
MinIO should automatically redirect you to the Keycloak login entry.
Upon successful authentication, Keycloak should redirect you back to the MinIO Console using either the originating Console URL or the Redirect URI if configured.

Generate Application Credentials using the Security Token Service (STS)

Applications using an S3-compatible SDK must specify credentials in the form of an access key and secret key.
The MinIO [[developers/security-token-service/AssumeRoleWithWebIdentity#minio-sts-assumerolewithwebidentity|AssumeRoleWithWebIdentity]] API returns the necessary temporary credentials, including a required session token, using a JWT returned by Keycloak after authentication.

You can test this workflow using the following sequence of HTTP calls and the `curl` utility:

Authenticate as a Keycloak user and retrieve the JWT token

```
curl -X POST "https://keycloak-url.example.net:8080/realms/REALM/protocol/openid-connect/token" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -d "username=USER" \
     -d "password=PASSWORD" \
     -d "grant_type=password" \
     -d "client_id=CLIENT" \
     -d "client_secret=SECRET"
```

Replace the `USER` and `PASSWORD` with the credentials of a Keycloak user on the `REALM`.

Replace the `CLIENT` and `SECRET` with the client ID and secret for the MinIO-specific Keycloak client on the `REALM`

You can process the results using `jq` or a similar JSON-formatting utility.
Extract the `access_token` field to retrieve the necessary access token.
Pay attention to the `expires_in` field to note the number of seconds before the token expires.

Generate MinIO Credentials using the `AssumeRoleWithWebIdentity` API

```
curl -X POST "https://minio-url.example.net:9000" \
     -H "Content-Type: application/x-www-form-urlencoded" \
     -d "Action=AssumeRoleWithWebIdentity" \
     -d "Version=2011-06-15" \
     -d "DurationSeconds=86000" \
     -d "WebIdentityToken=TOKEN"
```

Replace the `TOKEN` with the `access_token` value returned by Keycloak.

The API returns an XML document on success containing the following keys:

`Credentials.AccessKeyId` - the Access Key for the Keycloak User

`Credentials.SecretAccessKey` - the Secret Key for the Keycloak User

`Credentials.SessionToken` - the Session Token for the Keycloak User

`Credentials.Expiration` - the Expiration Date for the generated credentials

Test the Credentials

Use your preferred S3-compatible SDK to connect to MinIO using the generated credentials.

For example, the following Python code using the MinIO [[developers/python/minio-py#minio-python-quickstart|Python SDK]] connects to the MinIO deployment and returns a list of buckets:

```
from minio import Minio

client = MinIO(
   "minio-url.example.net:9000",
   access_key = "ACCESS_KEY",
   secret_key = "SECRET_KEY",
   session_token = "SESSION_TOKEN"
   secure = True
)

client.list_buckets()
```

Next Steps

Applications should implement the [[developers/security-token-service/AssumeRoleWithWebIdentity#minio-sts-assumerolewithwebidentity|STS AssumeRoleWithWebIdentity]] flow using their [[developers/minio-drivers#minio-drivers|SDK]] of choice.
When STS credentials expire, applications should have logic in place to regenerate the JWT token, STS token, and MinIO credentials before retrying and continuing operations.

Alternatively, users can generate [[administration/identity-access-management/minio-user-management#minio-id-access-keys|access keys]] through the MinIO Console for the purpose of creating long-lived API-key like access using their Keycloak credentials.

## Enable the Keycloak Admin REST API

MinIO supports using the Keycloak Admin REST API for checking if an authenticated user exists and is enabled on the Keycloak realm.
This functionality allows MinIO to more quickly remove access from previously authenticated Keycloak users.
Without this functionality, the earliest point in time that MinIO could disable access for a disabled or removed user is when the last retrieved authentication token expires.

This procedure assumes an existing MinIO deployment configured with Keycloak as an external identity manager.

### 1) Create the Necessary Client Scopes

Navigate to the Client scopes view and create a new scope:

Name

Set to a recognizable name for the scope (`minio-admin-API-access`)

Mappers

Select Configure a new mapper

Audience

Set the Name to any recognizable name for the mapping (`minio-admin-api-access-mapper`)

Included Client Audience

Set to `security-admin-console`.

Navigate to Clients and select the MinIO client

From Service account roles, select Assign role and assign the `admin` role

From Client scopes, select Add client scope and add the previously created scope

Navigate to Settings and ensure Authentication flow includes `Service accounts roles`.

### 2) Validate Admin API Access

You can validate the functionality by using the Admin REST API with the MinIO client credentials to retrieve a bearer token and user data:

Retrieve the bearer token:

```
curl -d "client_id=minio" \
     -d "client_secret=secretvalue" \
     -d "grant_type=password" \
     http://keycloak-url:port/admin/realms/REALM/protocol/openid-connect/token
```

Use the value returned as the `access_token` to access the Admin API:

```
curl -H "Authentication: Bearer ACCESS_TOKEN_VALUE" \
     http://keycloak-url:port/admin/realms/REALM/users/UUID
```

Replace `UUID` with the unique ID for the user which you want to retrieve.
The response should resemble the following:

```
{
   "id": "954de141-781b-4eaf-81bf-bf3751cdc5f2",
   "createdTimestamp": 1675866684976,
   "username": "minio-user-1",
   "enabled": true,
   "totp": false,
   "emailVerified": false,
   "firstName": "",
   "lastName": "",
   "attributes": {
      "policy": [
         "readWrite"
      ]
   },
   "disableableCredentialTypes": [],
   "requiredActions": [],
   "notBefore": 0,
   "access": {
      "manageGroupMembership": true,
      "view": true,
      "mapRoles": true,
      "impersonate": true,
      "manage": true
   }
}
```

MinIO would revoke access for an authenticated user if the returned value has `enabled: false` or `null` (user was removed from Keycloak).

### 3) Enable Keycloak Admin Support on MinIO

MinIO supports multiple methods for configuring Keycloak Admin API Support:

Using a terminal/shell and the [[reference/minio-mc/mc-idp-openid#command-mc.idp.openid|`mc idp openid`]] command

Using environment variables set prior to starting MinIO

CLI

You can use the [[reference/minio-mc/mc-idp-openid#mc.idp.openid.update|`mc idp openid update`]] command to modify the configuration settings for an existing Keycloak service.
You can alternatively include the following configuration settings when setting up Keycloak for the first time.
The command takes all supported [[reference/minio-server/settings/iam/openid#minio-open-id-config-settings|OpenID Configuration Settings]]:

```
mc idp openid update ALIAS KEYCLOAK_IDENTIFIER \
   vendor="keycloak" \
   keycloak_admin_url="https://keycloak-url:port/admin"
   keycloak_realm="REALM"
```

Replace `KEYCLOAK_IDENTIFIER` with the name of the configured Keycloak IDP.
You can use [[reference/minio-mc/mc-idp-openid#mc.idp.openid.ls|`mc idp openid ls`]] to view all configured IDP configurations on the MinIO deployment

Specify the Keycloak admin URL in the [[reference/minio-server/settings/iam/openid#mc-conf.identity_openid.keycloak_admin_url|`keycloak_admin_url`]] configuration setting

Specify the Keycloak Realm name in the [[reference/minio-server/settings/iam/openid#mc-conf.identity_openid.keycloak_realm|`keycloak_realm`]]

Environment Variables

Set the following [[reference/minio-server/settings/iam/openid#minio-server-envvar-external-identity-management-openid|environment variables]] in the appropriate configuration location, such as `/etc/default/minio`.

The following example code sets the minimum required environment variables related to enabling the Keycloak Admin API for an existing Keycloak configuration.
Replace the suffix `_PRIMARY_IAM` with the unique identifier for the target Keycloak configuration.

```
MINIO_IDENTITY_OPENID_VENDOR_PRIMARY_IAM="keycloak"
MINIO_IDENTITY_OPENID_KEYCLOAK_ADMIN_URL_PRIMARY_IAM="https://keycloak-url:port/admin"
MINIO_IDENTITY_OPENID_KEYCLOAK_REALM_PRIMARY_IAM="REALM"
```

Specify the Keycloak admin URL in the [[reference/minio-server/settings/iam/openid#envvar.MINIO_IDENTITY_OPENID_KEYCLOAK_ADMIN_URL|`MINIO_IDENTITY_OPENID_KEYCLOAK_ADMIN_URL`]]

Specify the Keycloak Realm name in the [[reference/minio-server/settings/iam/openid#envvar.MINIO_IDENTITY_OPENID_KEYCLOAK_REALM|`MINIO_IDENTITY_OPENID_KEYCLOAK_REALM`]]

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).