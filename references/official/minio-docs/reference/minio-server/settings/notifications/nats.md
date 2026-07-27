---
title: "NATS Notification Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/notifications/nats.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/notifications/nats"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "NATS Notification Settings — SILO (MinIO Object Storage)"
---
# NATS Notification Settings

NATS Streaming Deprecated

NATS Streaming is deprecated.
Migrate to [JetStream](https://docs.nats.io/nats-concepts/jetstream) instead.

The related MinIO configuration options and environment variables are deprecated.

This page documents settings for configuring an NATS service as a target for [[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket Notifications]].
See [[administration/monitoring/publish-events-to-nats#minio-bucket-notifications-publish-nats|Publish Events to NATS]] for a tutorial on using these settings.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Multiple NATS Targets

You can specify multiple NATS service endpoints by appending a unique identifier `_ID` for each set of related NATS settings on to the top level key.

### Example

For example, the following commands set two distinct NATS service endpoints as `PRIMARY` and `SECONDARY` respectively:

Environment Variables

```
export MINIO_NOTIFY_NATS_ENABLE_PRIMARY="on"
export MINIO_NOTIFY_NATS_ADDRESS_PRIMARY="nats-endpoint.example.net:4222"

export MINIO_NOTIFY_NATS_ENABLE_SECONDARY="on"
export MINIO_NOTIFY_NATS_ADDRESS_SECONDARY="nats-endpoint.example.net:4222"
```

With these settings, [`MINIO_NOTIFY_NATS_ENABLE_PRIMARY`](#envvar.MINIO_NOTIFY_NATS_ENABLE) indicates the environment variable is associated to an NATS service endpoint with ID of `PRIMARY`.

Configuration Settings

```
mc admin config set notify_nats:primary \
   address="nats-endpoint.example.com:4222" \
   subject="minioevents" \
   [ARGUMENT=VALUE ...]

mc admin config set notify_nats:secondary \
   address="nats-endpoint.example.com:4222" \
   subject="minioevents" \
   [ARGUMENT=VALUE ...]
```

## Settings

### Enable

Required

Environment Variable

MINIO_NOTIFY_NATS_ENABLE[](#envvar.MINIO_NOTIFY_NATS_ENABLE)

Specify `on` to enable publishing bucket notifications to an NATS service endpoint.

Defaults to `off`.

Configuration Setting

notify_nats[](#mc-conf.notify_nats)

The top-level configuration key for defining an NATS service endpoint for use with [[administration/monitoring/bucket-notifications#minio-bucket-notifications|MinIO bucket notifications]].

Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set or update an NATS service endpoint.
The [`address`](#mc-conf.notify_nats.address) and [`subject`](#mc-conf.notify_nats.subject) arguments are required for each target.
Specify additional optional arguments as a whitespace (`" "`)-delimited list.

```
mc admin config set notify_nats \
  address="nats-endpoint.example.com:4222" \
  subject="minioevents" \
  [ARGUMENT="VALUE"] ... \
```

### Address

Required

Environment Variable

MINIO_NOTIFY_NATS_ADDRESS[](#envvar.MINIO_NOTIFY_NATS_ADDRESS)

Configuration Setting

notify_nats address[](#mc-conf.notify_nats.address)

Specify the NATS service endpoint to which MinIO publishes bucket events.
For example, `nats-endpoint.example.com:4222`.

Changed in version RELEASE.2023-05-27T05-56-19Z: MinIO checks the health of the specified URL (if it is resolvable and reachable) prior to adding the target.
MinIO no longer blocks adding new notification targets if existing targets are offline.

### Subject

Required

Environment Variable

MINIO_NOTIFY_NATS_SUBJECT[](#envvar.MINIO_NOTIFY_NATS_SUBJECT)

Configuration Setting

notify_nats subject[](#mc-conf.notify_nats.subject)

Specify the subscription to which MinIO associates events published to the NATS endpoint.

### Username

Optional

Environment Variable

MINIO_NOTIFY_NATS_USERNAME[](#envvar.MINIO_NOTIFY_NATS_USERNAME)

Configuration Setting

notify_nats username[](#mc-conf.notify_nats.username)

Specify the username for connecting to the NATS service endpoint.

### Password

Optional

Environment Variable

MINIO_NOTIFY_NATS_PASSWORD[](#envvar.MINIO_NOTIFY_NATS_PASSWORD)

Configuration Setting

notify_nats password[](#mc-conf.notify_nats.password)

Specify the passport for connecting to the NATS service endpoint.

Changed in version RELEASE.2023-06-23T20-26-00Z: MinIO redacts this value when returned as part of [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]].

### Token

Optional

Environment Variable

MINIO_NOTIFY_NATS_TOKEN[](#envvar.MINIO_NOTIFY_NATS_TOKEN)

Configuration Setting

notify_nats token[](#mc-conf.notify_nats.token)

Specify the token for connecting to the NATS service endpoint.

Changed in version RELEASE.2023-06-23T20-26-00Z: MinIO redacts this value when returned as part of [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.get|`mc admin config get`]].

### User Credentials File

Optional

Environment Variable

MINIO_NOTIFY_NATS_USER_CREDENTIALS[](#envvar.MINIO_NOTIFY_NATS_USER_CREDENTIALS)

Configuration Setting

notify_nats user_credentials[](#mc-conf.notify_nats.user_credentials)

Specify the to the user credentials file <https://docs.nats.io/using-nats/developer/connecting/creds> to use to connect to the NATS service endpoint.

### TLS

Optional

Environment Variable

MINIO_NOTIFY_NATS_TLS[](#envvar.MINIO_NOTIFY_NATS_TLS)

Configuration Setting

notify_nats tls[](#mc-conf.notify_nats.tls)

Specify `on` to enable TLS connectivity to the NATS service endpoint.

### TLS Skip Verify

Optional

Environment Variable

MINIO_NOTIFY_NATS_TLS_SKIP_VERIFY[](#envvar.MINIO_NOTIFY_NATS_TLS_SKIP_VERIFY)

Configuration Setting

notify_nats tls_skip_verify[](#mc-conf.notify_nats.tls_skip_verify)

Enables or disables TLS verification of the NATS service endpoint TLS certificates.

Specify `on` to disable TLS verification (Default).

Specify `off` to enable TLS verification.

### Ping Interval

Optional

Environment Variable

MINIO_NOTIFY_NATS_PING_INTERVAL[](#envvar.MINIO_NOTIFY_NATS_PING_INTERVAL)

Configuration Setting

notify_nats ping_interval[](#mc-conf.notify_nats.ping_interval)

Specify the duration interval for client pings to the NATS server.
MinIO supports the following time units:

`s` - seconds, `"60s"`

`m` - minutes, `"5m"`

`h` - hours, `"1h"`

`d` - days, `"1d"`

### Jetstream

Optional

Environment Variable

MINIO_NOTIFY_NATS_JETSTREAM[](#envvar.MINIO_NOTIFY_NATS_JETSTREAM)

Configuration Setting

notify_nats jetstream[](#mc-conf.notify_nats.jetstream)

Specify `on` to enable JetStream support for streaming events to a NATS JetStream service endpoint.

### Streaming

Deprecated

Optional

Environment Variable

MINIO_NOTIFY_NATS_STREAMING[](#envvar.MINIO_NOTIFY_NATS_STREAMING)

Configuration Setting

notify_nats streaming[](#mc-conf.notify_nats.streaming)

Specify `on` to enable asynchronous publishing of events to the NATS service endpoint.

### Streaming Async

Deprecated

Optional

Environment Variable

MINIO_NOTIFY_NATS_STREAMING_ASYNC[](#envvar.MINIO_NOTIFY_NATS_STREAMING_ASYNC)

Configuration Setting

notify_nats streaming_async[](#mc-conf.notify_nats.streaming_async)

Specify `on` to enable asynchronous publishing of events to the NATS service endpoint.

### Max ACK Responses In Flight

Deprecated

Optional

Environment Variable

MINIO_NOTIFY_NATS_STREAMING_MAX_PUB_ACKS_IN_FLIGHT[](#envvar.MINIO_NOTIFY_NATS_STREAMING_MAX_PUB_ACKS_IN_FLIGHT)

Configuration Setting

notify_nats streaming_max_pub_acks_in_flight[](#mc-conf.notify_nats.streaming_max_pub_acks_in_flight)

Specify the number of messages to publish without waiting for an ACK response from the NATS service endpoint.

### Streaming Cluster ID

Deprecated

Optional

Environment Variable

MINIO_NOTIFY_NATS_STREAMING_CLUSTER_ID[](#envvar.MINIO_NOTIFY_NATS_STREAMING_CLUSTER_ID)

Configuration Setting

notify_nats streaming_cluster_id[](#mc-conf.notify_nats.streaming_cluster_id)

Specify the unique ID for the NATS streaming cluster.

### Cert Authority

Optional

Environment Variable

MINIO_NOTIFY_NATS_CERT_AUTHORITY[](#envvar.MINIO_NOTIFY_NATS_CERT_AUTHORITY)

Configuration Setting

notify_nats cert_authority[](#mc-conf.notify_nats.cert_authority)

Specify the path to the Certificate Authority chain used to sign the NATS service endpoint TLS certificates.

### Client Cert

Optional

Environment Variable

MINIO_NOTIFY_NATS_CLIENT_CERT[](#envvar.MINIO_NOTIFY_NATS_CLIENT_CERT)

Configuration Setting

notify_nats client_cert[](#mc-conf.notify_nats.client_cert)

Specify the path to the client certificate to use for performing mTLS authentication to the NATS service endpoint.

### Client Key

Optional

Environment Variable

MINIO_NOTIFY_NATS_CLIENT_KEY[](#envvar.MINIO_NOTIFY_NATS_CLIENT_KEY)

Configuration Setting

notify_nats client_key[](#mc-conf.notify_nats.client_key)

Specify the path to the client private key to use for performing mTLS authentication to the NATS service endpoint.

### Queue Directory

Optional

Environment Variable

MINIO_NOTIFY_NATS_QUEUE_DIR[](#envvar.MINIO_NOTIFY_NATS_QUEUE_DIR)

Configuration Setting

notify_nats queue_dir[](#mc-conf.notify_nats.queue_dir)

Specify the directory path to enable MinIO’s persistent event store for undelivered messages, such as `/opt/minio/events`.

MinIO stores undelivered events in the specified store while the NATS server/broker is offline and replays the stored events when connectivity resumes.

### Queue Limit

Optional

Environment Variable

MINIO_NOTIFY_NATS_QUEUE_LIMIT[](#envvar.MINIO_NOTIFY_NATS_QUEUE_LIMIT)

Configuration Setting

notify_nats queue_limit[](#mc-conf.notify_nats.queue_limit)

Specify the maximum limit for undelivered messages.
Defaults to `100000`.

### Comment

Optional

Environment Variable

MINIO_NOTIFY_NATS_COMMENT[](#envvar.MINIO_NOTIFY_NATS_COMMENT)

Configuration Setting

notify_nats comment[](#mc-conf.notify_nats.comment)

Specify a comment to associate with the NATS configuration.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).