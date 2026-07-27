---
title: "NSQ Notification Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/notifications/nsq.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/notifications/nsq"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "NSQ Notification Settings — SILO (MinIO Object Storage)"
---
# NSQ Notification Settings

This page documents settings for configuring an NSQ service as a target for [[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket Notifications]].
See [[administration/monitoring/publish-events-to-nsq#minio-bucket-notifications-publish-nsq|Publish Events to NSQ]] for a tutorial on using these settings.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Multiple NSQ Targets

You can specify multiple NSQ service endpoints by appending a unique identifier `_ID` to the end of the top level key for each set of related NSQ settings.
For example, the following commands set two distinct NSQ service endpoints as `PRIMARY` and `SECONDARY` respectively:

Environment Variables

```
export MINIO_NOTIFY_NSQ_ENABLE_PRIMARY="on"
export MINIO_NOTIFY_NSQ_NSQD_ADDRESS_PRIMARY="https://user:[email protected]:9200"
export MINIO_NOTIFY_NSQ_TOPIC_PRIMARY="bucketevents"

export MINIO_NOTIFY_NSQ_ENABLE_SECONDARY="on"
export MINIO_NOTIFY_NSQ_NSQD_ADDRESS_SECONDARY="https://user:[email protected]:9200"
export MINIO_NOTIFY_NSQ_TOPIC_SECONDARY="bucketevents"
```

Configuration Settings

```
mc admin config set notify_nsq:primary \
   nsqd_address="ENDPOINT" \
   topic="<string>" \
   [ARGUMENT="VALUE"] ... \

mc admin config set notify_nsq:secondary \
   nsqd_address="ENDPOINT" \
   topic="<string>" \
   [ARGUMENT="VALUE"] ... \
```

## Settings

### Enable

Required

Environment Variable

MINIO_NOTIFY_NSQ_ENABLE[](#envvar.MINIO_NOTIFY_NSQ_ENABLE)

Specify `on` to enable publishing bucket notifications to an NSQ endpoint.

Configuration Setting

notify_nsq[](#mc-conf.notify_nsq)

The top-level configuration key for defining an NSQ server/broker endpoint for use with [[administration/monitoring/bucket-notifications#minio-bucket-notifications|MinIO bucket notifications]].

Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set or update an NSQ server/broker endpoint.
The following arguments are required for each endpoint:

[`nsqd_address`](#mc-conf.notify_nsq.nsqd_address)

[`topic`](#mc-conf.notify_nsq.topic)

Specify additional optional arguments as a whitespace (`" "`)-delimited list.

```
mc admin config set notify_nsq                          \
   nsqd_address="https://nsq-endpoint.example.net:4150" \
   topic="<string>"                                     \
   [ARGUMENT="VALUE"] ...
```

### NSQ Daemon Server Address

Required

Environment Variable

MINIO_NOTIFY_NSQ_NSQD_ADDRESS[](#envvar.MINIO_NOTIFY_NSQ_NSQD_ADDRESS)

Configuration Setting

notify_nsq nsqd_address[](#mc-conf.notify_nsq.nsqd_address)

Specify the NSQ server address where the NSQ Daemon runs.
For example:

`https://nsq-endpoint.example.net:4150`

Changed in version RELEASE.2023-05-27T05-56-19Z: MinIO checks the health of the specified URL (if it is resolvable and reachable) prior to adding the target.
MinIO no longer blocks adding new notification targets if existing targets are offline.

### Topic

Required

Environment Variable

MINIO_NOTIFY_NSQ_TOPIC[](#envvar.MINIO_NOTIFY_NSQ_TOPIC)

Configuration Setting

notify_nsq topic[](#mc-conf.notify_nsq.topic)

Specify the name of the NSQ topic MinIO uses when publishing events to the broker.

### TLS

Optional

Environment Variable

MINIO_NOTIFY_NSQ_TLS[](#envvar.MINIO_NOTIFY_NSQ_TLS)

Configuration Setting

notify_nsq tls[](#mc-conf.notify_nsq.tls)

Specify `on` to enable TLS connectivity to the NSQ service broker.

### TLS Skip Verify

Optional

Environment Variable

MINIO_NOTIFY_NSQ_TLS_SKIP_VERIFY[](#envvar.MINIO_NOTIFY_NSQ_TLS_SKIP_VERIFY)

Configuration Setting

notify_nsq tls_skip_verify[](#mc-conf.notify_nsq.tls_skip_verify)

Enables or disables TLS verification of the NSQ service broker TLS certificates.

Specify `on` to disable TLS verification (Default).

Specify `off` to enable TLS verification.

### Queue Directory

Optional

Environment Variable

MINIO_NOTIFY_NSQ_QUEUE_DIR[](#envvar.MINIO_NOTIFY_NSQ_QUEUE_DIR)

Configuration Setting

notify_nsq queue_dir[](#mc-conf.notify_nsq.queue_dir)

Specify the directory path to enable MinIO’s persistent event store for undelivered messages, such as `/opt/minio/events`.

MinIO stores undelivered events in the specified store while the NSQ server/broker is offline and replays the stored events when connectivity resumes.

### Queue Limit

Optional

Environment Variable

MINIO_NOTIFY_NSQ_QUEUE_LIMIT[](#envvar.MINIO_NOTIFY_NSQ_QUEUE_LIMIT)

Configuration Setting

notify_nsq queue_limit[](#mc-conf.notify_nsq.queue_limit)

Specify the maximum limit for undelivered messages.
Defaults to `100000`.

### Comment

Optional

Environment Variable

MINIO_NOTIFY_NSQ_COMMENT[](#envvar.MINIO_NOTIFY_NSQ_COMMENT)

Configuration Setting

notify_nsq comment[](#mc-conf.notify_nsq.comment)

Specify a comment to associate with the NSQ configuration.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).