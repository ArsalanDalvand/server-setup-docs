---
title: "MinIO External Identity Management Plugin — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/identity-access-management/pluggable-authentication.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/identity-access-management/pluggable-authentication"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO External Identity Management Plugin — SILO (MinIO Object Storage)"
---
# MinIO External Identity Management Plugin

## Overview

The MinIO Identity Management Plugin provides a REST interface for offloading authentication to an external identity manager through a webhook service.

Once enabled, client applications use the `AssumeRoleWithCustomToken` STS API extension to generate access tokens for MinIO.
MinIO verifies this token by making a POST request to the configured plugin endpoint and uses the returned response to determine the authentication status of the client.

## Configuration Settings

You can configure the MinIO Identity Management Plugin using the following environment variables or configuration settings:

Environment Variables

Specify the following [[reference/minio-server/settings/iam/minio-identity-plugin#minio-server-envvar-external-identity-management-plugin|environment variables]] to each MinIO server in the deployment:

```
MINIO_IDENTITY_PLUGIN_URL="https://external-auth.example.net:8080/auth"
MINIO_IDENTITY_PLUGIN_ROLE_POLICY="consoleAdmin"

# All other envvars are optional
MINIO_IDENTITY_PLUGIN_TOKEN="Bearer TOKEN"
MINIO_IDENTITY_PLUGIN_ROLE_ID="external-auth-provider"
MINIO_IDENTITY_PLUGIN_COMMENT="External Identity Management using PROVIDER"
```

Configuration Settings

Set the following configuration settings using the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command:

```
mc admin config set identity_plugin \
   url="https://external-auth.example.net:8080/auth" \
   role_policy="consoleAdmin" \

   # All other config settings are optional
   token="Bearer TOKEN" \
   role_id="external-auth-provider" \
   comment="External Identity Management using PROVIDER"
```

## Authentication and Authorization Flow

The login flow for an application is as follows:

Make a POST request using the [[developers/security-token-service/AssumeRoleWithCustomToken#minio-sts-assumerolewithcustomtoken|AssumeRoleWithCustomToken]] API.

The request includes a token used by the configured external identity manager for authenticating the client.

MinIO makes a POST call to the configured identity plugin URL using the token specified to the STS API.

On successful authentication, the identity manager returns a `200 OK` response with an `application/json` content-type and body with the following structure:

```
{
   "user": "<string>",
   "maxValiditySeconds": 3600,
   "claims": {"KEY": "VALUE", ...}
}
```

`user`

The owner of the requested credentials

`maxValiditySeconds`

The maximum allowed expiry duration for the returned credentials

`claims`

A JSON string of `"key": "value"` pair claims associated with the requested credentials.
MinIO reserves and ignores the `exp`, `parent`, and `sub` claims objects if present.

MinIO returns a response to the STS API request that includes temporary credentials for use with making authenticated requests.

If the identity manager rejects the authentication request or otherwise encounters an error, the response must return a `403 FORBIDDEN` HTTP status code with an `application/json` content-type and body with the following structure:

```
{
     "reason": "<string>"
}
```

The `"reason"` field should include the reason for the 403.

## Creating Policies to Match Claims

Use the [[reference/minio-mc-admin/mc-admin-policy#command-mc.admin.policy|`mc admin policy`]] command to create policies that match one or more claim values.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).