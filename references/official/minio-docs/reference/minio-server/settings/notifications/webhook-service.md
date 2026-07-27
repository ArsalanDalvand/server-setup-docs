---
title: "Webhook Service Notification Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/notifications/webhook-service.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/notifications/webhook-service"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Webhook Service Notification Settings — SILO (MinIO Object Storage)"
---
# Webhook Service Notification Settings

This page documents settings for configuring an Webhook service as a target for [[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket Notifications]].
See [[administration/monitoring/publish-events-to-webhook#minio-bucket-notifications-publish-webhook|Publish Events to Webhook]] for a tutorial on using these settings.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Multiple Webhook Service Targets

You can specify multiple Webhook service endpoints by appending a unique identifier `_ID` for each set of related Webhook settings on to the top level key.
For example, the following commands set two distinct Webhook service endpoints as `PRIMARY` and `SECONDARY` respectively:

Environment Variables

```
export MINIO_NOTIFY_WEBHOOK_ENABLE_PRIMARY="on"
export MINIO_NOTIFY_WEBHOOK_ENDPOINT_PRIMARY="https://webhook1.example.net"

export MINIO_NOTIFY_WEBHOOK_ENABLE_SECONDARY="on"
export MINIO_NOTIFY_WEBHOOK_ENDPOINT_SECONDARY="https://webhook1.example.net"
```

Configuration Settings

```
mc admin config set notify_webhook:primary \
   endpoint="https://webhook1.example.net"
   [ARGUMENT=VALUE ...]

mc admin config set notify_webhook:secondary \
   endpoint="https://webhook2.example.net
   [ARGUMENT=VALUE ...]
```

## Settings

### Enable

Required

Environment Variable

MINIO_NOTIFY_WEBHOOK_ENABLE[](#envvar.MINIO_NOTIFY_WEBHOOK_ENABLE)

Specify `on` to enable publishing bucket notifications to a Webhook service endpoint.

Defaults to `off`.

Configuration Setting

notify_webhook[](#mc-conf.notify_webhook)

The top-level configuration key for defining an Webhook service endpoint for use with [[administration/monitoring/bucket-notifications#minio-bucket-notifications|MinIO bucket notifications]].

Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set or update an Webhook service endpoint.
The [`endpoint`](#mc-conf.notify_webhook.endpoint) argument is required for each target.
Specify additional optional arguments as a whitespace (`" "`)-delimited list.

```
mc admin config set notify_webhook \
  endpoint="https://webhook.example.net"
  [ARGUMENT="VALUE"] ... \
```

### Endpoint

Required

Environment Variable

MINIO_NOTIFY_WEBHOOK_ENDPOINT[](#envvar.MINIO_NOTIFY_WEBHOOK_ENDPOINT)

Configuration Setting

notify_webhook endpoint[](#mc-conf.notify_webhook.endpoint)

Specify the URL for the webhook service.

Changed in version RELEASE.2023-05-27T05-56-19Z: MinIO checks the health of the specified URL (if it is resolvable and reachable) prior to adding the target.
MinIO no longer blocks adding new notification targets if existing targets are offline.

### Auth Token

Required

Environment Variable

MINIO_NOTIFY_WEBHOOK_AUTH_TOKEN[](#envvar.MINIO_NOTIFY_WEBHOOK_AUTH_TOKEN)

An authentication token of the appropriate type for the endpoint.
Omit for endpoints which do not require authentication.

To allow for a variety of token types, MinIO creates the request authentication header using the value exactly as specified.
Depending on the endpoint, you may need to include additional information.

For example, for a Bearer token, prepend `Bearer`:

```
export MINIO_NOTIFY_WEBHOOK_AUTH_TOKEN_myendpoint="Bearer 1a2b3c4f5e"
```

Modify the value according to the endpoint requirements.
A custom authentication format could resemble the following:

```
export MINIO_NOTIFY_WEBHOOK_AUTH_TOKEN_xyz="ServiceXYZ 1a2b3c4f5e"
```

Consult the documentation for the desired service for more details.

Configuration Setting

notify_webhook auth_token[](#mc-conf.notify_webhook.auth_token)

An authentication token of the appropriate type for the endpoint.
Omit for endpoints which do not require authentication.

To allow for a variety of token types, MinIO creates the request authentication header using the value exactly as specified.
Depending on the endpoint, you may need to include additional information.

For example, for a Bearer token, prepend `Bearer`:

```
mc admin config set myminio notify_webhook   \
   endpoint="https://webhook-1.example.net"  \
      auth_token="Bearer 1a2b3c4f5e"
```

Modify the value according to the endpoint requirements.
A custom authentication format could resemble the following:

```
mc admin config set myminio notify_webhook   \
      endpoint="https://webhook-1.example.net"  \
      auth_token="ServiceXYZ 1a2b3c4f5e"
```

Consult the documentation for the desired service for more details.

Changed in version RELEASE.2023-06-23T20-26-00Z.

MinIO redacts this value when returned as part of [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]].

### Queue Directory

Optional

Environment Variable

MINIO_NOTIFY_WEBHOOK_QUEUE_DIR[](#envvar.MINIO_NOTIFY_WEBHOOK_QUEUE_DIR)

Configuration Setting

notify_webhook queue_dir[](#mc-conf.notify_webhook.queue_dir)

Specify the directory path to enable MinIO’s persistent event store for undelivered messages, such as `/opt/minio/events`.

MinIO stores undelivered events in the specified store while the webhook service is offline and replays the stored events when connectivity resumes.

### Queue Limit

Optional

Environment Variable

MINIO_NOTIFY_WEBHOOK_QUEUE_LIMIT[](#envvar.MINIO_NOTIFY_WEBHOOK_QUEUE_LIMIT)

Configuration Setting

notify_webhook queue_limit[](#mc-conf.notify_webhook.queue_limit)

Specify the maximum limit for undelivered messages.
Defaults to `100000`.

### Client Certificate

Optional

Environment Variable

MINIO_NOTIFY_WEBHOOK_CLIENT_CERT[](#envvar.MINIO_NOTIFY_WEBHOOK_CLIENT_CERT)

Configuration Setting

notify_webhook client_cert[](#mc-conf.notify_webhook.client_cert)

Specify the path to the client certificate to use for performing mTLS authentication to the webhook service.

### Client Key

Optional

Environment Variable

MINIO_NOTIFY_WEBHOOK_CLIENT_KEY[](#envvar.MINIO_NOTIFY_WEBHOOK_CLIENT_KEY)

Configuration Setting

notify_webhook client_key[](#mc-conf.notify_webhook.client_key)

Specify the path to the client private key to use for performing mTLS authentication to the webhook service.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).