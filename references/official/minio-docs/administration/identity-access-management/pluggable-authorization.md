---
title: "MinIO External Access Management Plugin — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/identity-access-management/pluggable-authorization.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/identity-access-management/pluggable-authorization"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "MinIO External Access Management Plugin — SILO (MinIO Object Storage)"
---
# MinIO External Access Management Plugin

## Overview

The MinIO Access Management Plugin provides a `REST` interface for offloading authorization through a webhook service.

Once enabled, MinIO sends the request and credential details for every API call to the configured external HTTP(S) endpoint and looks for a response of `ALLOW` or `DENY`.
MinIO can therefore delegate the access management to the external system instead of relying on S3 [[administration/identity-access-management/policy-based-access-control#minio-policy|policy based access control]].

## Configuration Settings

You can configure the MinIO External Access Management Plugin using the following environment variables or configuration settings.

Environment Variables

Specify the following [[reference/minio-server/settings/iam/minio-access-plugin#minio-server-envvar-external-access-management-plugin|environmental variables]] to each MinIO server in the deployment:

```
MINIO_POLICY_PLUGIN_URL="https://external-authz.example.net:8080/authz"

# All other envvars are optional
MINIO_POLICY_PLUGIN_AUTH_TOKEN="Bearer TOKEN"
MINIO_POLICY_PLUGIN_ENABLE_HTTP2="OFF"
MINIO_POLICY_PLUGIN_COMMENT="External Access Management using PROVIDER"
```

Configuration Settings

Set the following configuration settings using the [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] command:

```
mc admin config set policy_plugin \
   url="https://external-authz.example.net:8080/authz" \

   # All other config settings are optional
   auth_token="Bearer TOKEN" \
   enable_http2="off" \
   comment="External Access Management using PROVIDER"
```

## Authentication and Authorization Flow

The login flow for an application is as follows:

The client includes authentication information as part of performing the API call

The configured Identity Manager authenticates the client

MinIO makes a `POST` call to the configured access management plugin URL which includes the context of the API call and authentication data

On successful authorization, the access manager returns a `200 OK` response with a JSON body of either `result true` or `"result" : { "allow" : true }`:

If the access manager rejects the authorization request, MinIO automatically blocks and denies the API call.

## Request Body Example

The following JSON resembles the request body sent as part of the POST to the configured access manager webhook.

```
{
   "input": {
      "account": "minio",
      "groups": null,
      "action": "s3:ListBucket",
      "bucket": "test",
      "conditions": {
         "Authorization": [
         "AWS4-HMAC-SHA256 Credential=minio/20220507/us-east-1/s3/aws4_request, SignedHeaders=host;x-amz-content-sha256;x-amz-date, Signature=62012db6c47d697620cf6c68f0f45f6e34894589a53ab1faf6dc94338468c78a"
         ],
         "CurrentTime": [ "2022-05-07T18:31:41Z" ],
         "Delimiter": [ "/" ],
         "EpochTime": [
         "1651948301"
         ],
         "Prefix": [ "" ],
         "Referer": [ "" ],
         "SecureTransport": [ "false" ],
         "SourceIp": [ "127.0.0.1" ],
         "User-Agent": [ "MinIO (linux; amd64) minio-go/v7.0.24 mc/DEVELOPMENT.2022-04-20T23-07-53Z" ],
         "UserAgent": [ "MinIO (linux; amd64) minio-go/v7.0.24 mc/DEVELOPMENT.2022-04-20T23-07-53Z" ],
         "X-Amz-Content-Sha256": [ "e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855" ],
         "X-Amz-Date": [ "20220507T183141Z" ],
         "authType": [ "REST-HEADER" ],
         "principaltype": [ "Account" ],
         "signatureversion": [ "AWS4-HMAC-SHA256" ],
         "userid": [ "minio" ],
         "username": [ "minio" ],
         "versionid": [ "" ]
      },
      "owner": true,
      "object": "",
      "claims": {},
      "denyOnly": false
   }
}
```

## Response Body Example

MinIO requires the response body from the Access Management service meet one of the two following formats:

```
{ "result" : true }

{ "result" : { "allow" : true } }
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).