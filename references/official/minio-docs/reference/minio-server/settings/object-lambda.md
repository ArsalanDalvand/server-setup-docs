---
title: "Object Lambda function settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/object-lambda.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/object-lambda"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Object Lambda function settings — SILO (MinIO Object Storage)"
---
# Object Lambda function settings

This page describes the settings available to configure MinIO to publish data to an HTTP webhook endpoint and trigger an Object Lambda function.
See [[developers/transforms-with-object-lambda#developers-object-lambda|Transforms with Object Lambda]] for more complete documentation and tutorials on using these settings.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Enable

Environment Variable

MINIO_LAMBDA_WEBHOOK_ENABLE[](#envvar.MINIO_LAMBDA_WEBHOOK_ENABLE)

Specify `"on"` to enable the Object Lambda webhook endpoint for a handler function.

Requires specifying [`MINIO_LAMBDA_WEBHOOK_ENDPOINT`](#envvar.MINIO_LAMBDA_WEBHOOK_ENDPOINT).

You can specify multiple webhooks as Lambda targets by appending a unique identifier for each Object Lambda function.
For example, the following command enables two distinct Object Lambda webhook endpoints:

```
export MINIO_LAMBDA_WEBHOOK_ENABLE_myfunction="on"
export MINIO_LAMBDA_WEBHOOK_ENABLE_yourfunction="on"
```

Configuration Setting

lambda_webhook enable[](#mc-conf.lambda_webhook.enable)

Optional

Specify `"on"` to enable the Object Lambda webhook endpoint for a handler function.
Requires specifying [`endpoint`](#mc-conf.lambda_webhook.endpoint).

Example:

```
mc admin config set myminio lambda_webhook:myfunction endpoint="https://example.com/" enable=on
```

## Endpoint

Environment Variable

MINIO_LAMBDA_WEBHOOK_ENDPOINT[](#envvar.MINIO_LAMBDA_WEBHOOK_ENDPOINT)

The HTTP endpoint of the lambda webhook for the handler function.

You can specify multiple webhook endpoints as Lambda targets by appending a unique identifier for each Object Lambda function.
For example, the following command sets two distinct Object Lambda webhook endpoints:

```
export MINIO_LAMBDA_WEBHOOK_ENDPOINT_myfunction="http://webhook-1.example.com"
export MINIO_LAMBDA_WEBHOOK_ENDPOINT_yourfunction="http://webhook-2.example.com"
```

Configuration Setting

lambda_webhook endpoint[](#mc-conf.lambda_webhook.endpoint)

Optional

The HTTP endpoint of the lambda webhook for the handler function.

## Auth token

Environment Variable

MINIO_LAMBDA_WEBHOOK_AUTH_TOKEN[](#envvar.MINIO_LAMBDA_WEBHOOK_AUTH_TOKEN)

Specify the opaque string or JWT authorization token to use for authenticating to the lambda webhook service.

You can specify the token for multiple Lambda targets by appending a unique identifier for each Object Lambda function.
For example, the following command configures a token for two distinct Object Lambda webhook endpoints:

```
export MINIO_LAMBDA_WEBHOOK_AUTH_TOKEN_myfunction="1a2b3c4d5e"
export MINIO_LAMBDA_WEBHOOK_AUTH_TOKEN_yourfunction="1a2b3c4d5e"
```

Changed in version RELEASE.2023-06-23T20-26-00Z: MinIO redacts this value when returned as part of [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]].

Configuration Setting

lambda_webhook auth_token[](#mc-conf.lambda_webhook.auth_token)

Optional

Specify the opaque string or JWT authorization token to use for authenticating to the lambda webhook service.

Changed in version RELEASE.2023-06-23T20-26-00Z: MinIO redacts this value when returned as part of [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]].

## Client cert

Environment Variable

MINIO_LAMBDA_WEBHOOK_CLIENT_CERT[](#envvar.MINIO_LAMBDA_WEBHOOK_CLIENT_CERT)

Specify the path to the client certificate to use for performing mTLS authentication to the lambda webhook service.

You can specify the client cert for multiple Lambda targets by appending a unique identifier for each Object Lambda function.
For example, the following command configures a cert for two distinct Object Lambda webhook endpoints:

```
export MINIO_LAMBDA_WEBHOOK_CLIENT_CERT_myfunction="/path/to/cert1"
export MINIO_LAMBDA_WEBHOOK_CLIENT_CERT_yourfunction="/path/to/cert2"
```

Configuration Setting

lambda_webhook client_cert[](#mc-conf.lambda_webhook.client_cert)

Optional

Specify the path to the client certificate to use for performing mTLS authentication to the lambda webhook service.

## Client key

Environment Variable

MINIO_LAMBDA_WEBHOOK_CLIENT_KEY[](#envvar.MINIO_LAMBDA_WEBHOOK_CLIENT_KEY)

Specify the path to the private key to use for performing mTLS authentication to the lambda webhook service.

You can specify the client key for multiple Lambda targets by appending a unique identifier for each Object Lambda function.
For example, the following command configures a key for two distinct Object Lambda webhook endpoints:

```
export MINIO_LAMBDA_WEBHOOK_CLIENT_KEY_myfunction="/path/to/key1"
export MINIO_LAMBDA_WEBHOOK_CLIENT_KEY_yourfunction="/path/to/key2"
```

Configuration Setting

lambda_webhook client_key[](#mc-conf.lambda_webhook.client_key)

Optional

Specify the path to the private key to use for performing mTLS authentication to the lambda webhook service.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).