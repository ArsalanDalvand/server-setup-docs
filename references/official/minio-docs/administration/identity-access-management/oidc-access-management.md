---
title: "OpenID Connect Access Management — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/identity-access-management/oidc-access-management.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/identity-access-management/oidc-access-management"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "OpenID Connect Access Management — SILO (MinIO Object Storage)"
---
# OpenID Connect Access Management

MinIO supports using an OpenID Connect (OIDC) compatible IDentity Provider (IDP)
such as Okta, KeyCloak, Dex, Google, or Facebook for external management of user
identities.

For identities managed by the external OpenID Connect (OIDC) compatible provider, MinIO can use either of two methods to assign policies to the authenticated user.

Use the [JSON Web Token claim](https://datatracker.ietf.org/doc/html/rfc7519#section-4) returned as part of the OIDC authentication flow to identify the [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] to assign to the authenticated user.

Use the `RoleArn` specified in the authorization request to assign the policies attached to the provider’s RolePolicy.

MinIO by default denies access to all actions or resources not explicitly allowed by a user’s assigned or inherited [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]].
Users managed by an OIDC provider must specify the necessary policies as part of the JWT claim. If the user JWT claim has no matching MinIO policies, that user has no permissions to access any action or resource on the MinIO deployment.

The specific claim which MinIO looks for is configured as part of [[operations/external-iam#minio-external-iam-oidc|deploying the cluster with OIDC identity management]]. This page focuses on creating MinIO policies to match the configured OIDC claims.

## Authentication and Authorization Flow

MinIO supports two OIDC authentication and authorization flows:

The RolePolicy flow sets the assigned policies for an authenticated user in the MinIO configuration.

MinIO recommends using the RolePolicy method for authenticating with an OpenID provider.

The JWT flow sets the assigned policies for an authenticated user as part of the OIDC configuration.

MinIO supports multiple OIDC provider configurations.
However, you can configure only one JWT claim-based OIDC provider per deployment.
All other providers must use RolePolicy.

### RolePolicy and RoleArn

With a RolePolicy, all clients which generate an STS credential using a given RoleArn receive the [[administration/identity-access-management/policy-based-access-control#minio-policy|policy or policies]] associated to the RolePolicy configuration for that RoleArn.

You can use  [[administration/identity-access-management/policy-based-access-control#minio-policy-variables-oidc|OpenID Policy Variables]] to create policies that programmatically manage what each individual user has access to.

The login flow for an application using OIDC credentials with a RolePolicy claim flow is as follows:

Create an OIDC Configuration.

Record the RoleArn assigned to the configuration either at time of creation or at MinIO start.
Use this RoleArn with the [[developers/security-token-service/AssumeRoleWithWebIdentity#minio-sts-assumerolewithwebidentity|AssumeRoleWithWebIdentity]] STS API.

Create a RolePolicy to use with the RoleArn.
Use either the [[reference/minio-server/settings/iam/openid#envvar.MINIO_IDENTITY_OPENID_ROLE_POLICY|`MINIO_IDENTITY_OPENID_ROLE_POLICY`]] environment variable or the [[reference/minio-server/settings/iam/openid#mc-conf.identity_openid.role_policy|`identity_openid role_policy`]] configuration setting to define the list of policies to use for the provider

Users select the configured OIDC provider when logging in to MinIO.

Users complete authentication to the configured OIDC provider and redirect back to MinIO.

MinIO only supports the [OpenID Authorization Code Flow](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth).
Authentication using Implicit Flow is not supported.

MinIO verifies the `RoleArn` in the API call and checks for the [RolePolicy](#minio-external-identity-management-openid-access-control) to use.
Any authentication request with the RoleArn receives the same policy access permissions.

MinIO returns temporary credentials in the STS API response in the form of an access key, secret key, and session token.
The credentials have permissions matching those policies specified in the RolePolicy.

Applications use the temporary credentials returned by the STS endpoint to perform authenticated S3 operations on MinIO.

### JSON Web Token Claim

Using JSON Web Tokens allows you to have individual assignment of policies.
However, the use of web tokens also comes at the increased cost of managing multiple policies for separate claims.

The login flow for an application using OIDC
credentials with a JSON Web Token Claim flow is as follows:

Authenticate to the configured OIDC
provider and retrieve a [JSON Web Token (JWT)](https://jwt.io/introduction).

MinIO only supports the [OpenID Authorization Code Flow](https://openid.net/specs/openid-connect-core-1_0.html#CodeFlowAuth).
Authentication using Implicit Flow is not supported.

Specify the JWT to the MinIO Security Token Service
(STS) [[developers/security-token-service/AssumeRoleWithWebIdentity#minio-sts-assumerolewithwebidentity|AssumeRoleWithWebIdentity]] API endpoint.

MinIO verifies the JWT against the configured OIDC provider.

If the JWT is valid, MinIO checks for a [claim](#minio-external-identity-management-openid-access-control) specifying a list
of one or more [[administration/identity-access-management/policy-based-access-control#minio-policy|policies]] to assign to the
authenticated user. MinIO defaults to checking the `policy` claim.

MinIO returns temporary credentials in the STS API response in the form of an
access key, secret key, and session token. The credentials have
permissions matching those policies specified in the JWT claim.

Applications use the temporary credentials returned by the STS endpoint to
perform authenticated S3 operations on MinIO.

MinIO provides an example Go application [web-identity.go](https://github.com/minio/minio/blob/master/docs/sts/web-identity.go) that handles the full login flow.

#### Identifying the JWT Claim Value

MinIO uses the JWT token returned as part of the OIDC authentication flow to identify the specific policies to assign to the authenticated user.

You can use a [JWT Debugging tool](https://jwt.io/) to decode the returned JWT token and validate that the user attributes include the required claims.

See [RFC 7519: JWT Claim](https://datatracker.ietf.org/doc/html/rfc7519#section-4) for more information on JWT claims.

Defer to the documentation for your preferred OIDC provider for instructions on configuring user claims.

## Creating Policies to Match Claims

Use the [[reference/minio-mc-admin/mc-admin-policy#command-mc.admin.policy|`mc admin policy`]] command to create policies that match one or more claim values.

## OIDC Policy Variables

The following table contains a list of supported policy variables for use in authorizing [OIDC-managed users](#minio-external-identity-management-openid).

Each variable corresponds to a claim returned as part of the authenticated user’s JWT token:

Variable

Description

`jwt:sub`

Returns the `sub` claim for the user.

`jwt:iss`

Returns the Issuer Identifier claim from the ID token.

`jwt:aud`

Returns the Audience claim from the ID token.

`jwt:jti`

Returns the JWT ID claim from the client authentication information.

`jwt:upn`

Returns the User Principal Name claim from the client authentication information.

`jwt:name`

Returns the `name` claim for the user.

`jwt:groups`

Returns the `groups` claim for the user.

`jwt:given_name`

Returns the `given_name` claim for the user.

`jwt:family_name`

Returns the `family_name` claim for the user.

`jwt:middle_name`

Returns the `middle_name` claim for the user.

`jwt:nickname`

Returns the `nickname` claim for the user.

`jwt:preferred_username`

Returns the `preferred_username` claim for the user.

`jwt:profile`

Returns the `profile` claim for the user.

`jwt:picture`

Returns the `picture` claim for the user.

`jwt:website`

Returns the `website` claim for the user.

`jwt:email`

Returns the `email` claim for the user.

`jwt:gender`

Returns the `gender` claim for the user.

`jwt:birthdate`

Returns the `birthdate` claim for the user.

`jwt:phone_number`

Returns the `phone_number` claim for the user.

`jwt:address`

Returns the `address` claim for the user.

`jwt:scope`

Returns the `scope` claim for the user.

`jwt:client_id`

Returns the `client_id` claim for the user.

See the [OpenID Connect Core 1.0](https://openid.net/specs/openid-connect-core-1_0.html) document for more information on these scopes.
Your OIDC provider of choice may have more specific documentation.

For example, the following policy uses variables to substitute the authenticated user’s `preferred_username` as part of the `Resource` field such that the user can only access those prefixes which match their username:

```
{
"Version": "2012-10-17",
"Statement": [
      {
         "Action": ["s3:ListBucket"],
         "Effect": "Allow",
         "Resource": ["arn:aws:s3:::mybucket"],
         "Condition": {"StringLike": {"s3:prefix": ["${jwt:preferred_username}/*"]}}
      },
      {
         "Action": [
         "s3:GetObject",
         "s3:PutObject"
         ],
         "Effect": "Allow",
         "Resource": ["arn:aws:s3:::mybucket/${jwt:preferred_username}/*"]
      }
   ]
}
```

MinIO replaces the `${jwt:preferred_username}` variable in the `Resource` field with the value of the `preferred_username` in the JWT token.
MinIO then evaluates the policy and grants or revokes access to the requested API and resource.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).