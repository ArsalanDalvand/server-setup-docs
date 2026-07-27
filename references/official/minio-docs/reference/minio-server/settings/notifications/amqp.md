---
title: "AMQP Notification Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/notifications/amqp.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/notifications/amqp"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "AMQP Notification Settings — SILO (MinIO Object Storage)"
---
# AMQP Notification Settings

This page documents settings for configuring an AMQP service as a target for [[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket Notifications]].
See [[administration/monitoring/publish-events-to-amqp#minio-bucket-notifications-publish-amqp|Publish Events to AMQP (RabbitMQ)]] for a tutorial on using these settings.

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Multiple AMQP Targets

You can specify multiple AMQP service endpoints by appending a unique identifier `_ID` for each set of related AMQP settings to the top level key.

### Examples

For example, the following commands set two distinct AMQP service endpoints as `PRIMARY` and `SECONDARY` respectively:

Environment Variables

```
export MINIO_NOTIFY_AMQP_ENABLE_PRIMARY="on"
export MINIO_NOTIFY_AMQP_URL_PRIMARY="amqp://user:[email protected]:5672"

export MINIO_NOTIFY_AMQP_ENABLE_SECONDARY="on"
export MINIO_NOTIFY_AMQP_URL_SECONDARY="amqp://user:[email protected]:5672"
```

For example, [`MINIO_NOTIFY_AMQP_ENABLE_PRIMARY`](#envvar.MINIO_NOTIFY_AMQP_ENABLE) indicates the environment variable is associated to an AMQP service endpoint with ID of `PRIMARY`.

Configuration Settings

```
mc admin config set notify_amqp:primary \
   url="user:password@amqp://amqp-endpoint.example.net:5672" [ARGUMENT=VALUE ...]

mc admin config set notify_amqp:secondary \
   url="user:password@amqp://amqp-endpoint.example.net:5672" [ARGUMENT=VALUE ...]
```

Notice that for configuration settings, the unique identifier appends to `amqp` only, not to each individual argument.

## Settings

### Enable

Environment Variable

MINIO_NOTIFY_AMQP_ENABLE[](#envvar.MINIO_NOTIFY_AMQP_ENABLE)

Requires specifying [`MINIO_NOTIFY_AMQP_URL`](#envvar.MINIO_NOTIFY_AMQP_URL) if set to `on`.

Specify `on` to enable publishing bucket notifications to an AMQP endpoint.

Defaults to `off`.

Configuration Setting

notify_amqp[](#mc-conf.notify_amqp)

The top-level configuration key for defining an AMQP service endpoint for use with [[administration/monitoring/bucket-notifications#minio-bucket-notifications|MinIO bucket notifications]].

Use [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]] to set or update an AMQP service endpoint.
The [`url`](#mc-conf.notify_amqp.url) argument is required for each target.
Specify additional optional arguments as a whitespace (`" "`)-delimited list.

```
mc admin config set notify_amqp \
  url="amqp://user:password@endpoint:port" \
  [ARGUMENT="VALUE"] ...
```

### URL

Required

Environment Variable

MINIO_NOTIFY_AMQP_URL[](#envvar.MINIO_NOTIFY_AMQP_URL)

Configuration Setting

notify_amqp url[](#mc-conf.notify_amqp.url)

Specify the AMQP server endpoint to which MinIO publishes bucket events.
For example, `amqp://myuser:mypassword@localhost:5672`.

Changed in version RELEASE.2023-05-27T05-56-19Z: MinIO checks the health of the specified URL (if it is resolvable and reachable) prior to adding the target.
MinIO no longer blocks adding new notification targets if existing targets are offline.

### Exchange

Optional

Environment Variable

MINIO_NOTIFY_AMQP_EXCHANGE[](#envvar.MINIO_NOTIFY_AMQP_EXCHANGE)

Configuration Setting

notify_amqp exchange[](#mc-conf.notify_amqp.exchange)

Specify the name of the AMQP exchange to use.

### Exchange Type

Optional

Environment Variable

MINIO_NOTIFY_AMQP_EXCHANGE_TYPE[](#envvar.MINIO_NOTIFY_AMQP_EXCHANGE_TYPE)

Configuration Setting

notify_amqp exchange_type[](#mc-conf.notify_amqp.exchange_type)

Specify the type of the AMQP exchange.

### Routing Key

Optional

Environment Variable

MINIO_NOTIFY_AMQP_ROUTING_KEY[](#envvar.MINIO_NOTIFY_AMQP_ROUTING_KEY)

Configuration Setting

notify_amqp routing_key[](#mc-conf.notify_amqp.routing_key)

Specify the routing key for publishing events.

### Mandatory

Optional

Environment Variable

MINIO_NOTIFY_AMQP_MANDATORY[](#envvar.MINIO_NOTIFY_AMQP_MANDATORY)

Configuration Setting

notify_amqp mandatory[](#mc-conf.notify_amqp.mandatory)

Specify `off` to ignore undelivered messages errors.
Defaults to `on`.

### Durable

Optional

Environment Variable

MINIO_NOTIFY_AMQP_DURABLE[](#envvar.MINIO_NOTIFY_AMQP_DURABLE)

Configuration Setting

notify_amqp durable[](#mc-conf.notify_amqp.durable)

Specify `on` to persist the message queue across broker restarts.
Defaults to `off`.

### No Wait

Optional

Environment Variable

MINIO_NOTIFY_AMQP_NO_WAIT[](#envvar.MINIO_NOTIFY_AMQP_NO_WAIT)

Configuration Setting

notify_amqp no_wait[](#mc-conf.notify_amqp.no_wait)

Specify `on` to enable non-blocking message delivery.
Defaults to `off`.

### Internal

Optional

Environment Variable

MINIO_NOTIFY_AMQP_INTERNAL[](#envvar.MINIO_NOTIFY_AMQP_INTERNAL)

Configuration Setting

notify_amqp internal[](#mc-conf.notify_amqp.internal)

Specify `on` to use the exchange only if it is bound to other exchanges.
See the RabbitMQ documentation on [Exchange to Exchange Bindings](https://www.rabbitmq.com/e2e.html) for more information on AMQP exchange binding.

### Auto Deleted

Optional

Environment Variable

MINIO_NOTIFY_AMQP_AUTO_DELETED[](#envvar.MINIO_NOTIFY_AMQP_AUTO_DELETED)

Configuration Setting

notify_amqp auto_deleted[](#mc-conf.notify_amqp.auto_deleted)

Specify `on` to automatically delete the message queue if there are no consumers.
Defaults to `off`.

### Delivery Mode

Optional

Environment Variable

MINIO_NOTIFY_AMQP_DELIVERY_MODE[](#envvar.MINIO_NOTIFY_AMQP_DELIVERY_MODE)

Configuration Setting

notify_amqp delivery_mode[](#mc-conf.notify_amqp.delivery_mode)

Specify `1` for set the delivery mode to non-persistent queue.

Specify `2` to set the delivery mode to persistent queue.

### Queue Directory

Optional

Environment Variable

MINIO_NOTIFY_AMQP_QUEUE_DIR[](#envvar.MINIO_NOTIFY_AMQP_QUEUE_DIR)

Configuration Setting

notify_amqp queue_dir[](#mc-conf.notify_amqp.queue_dir)

Specify the directory path to enable MinIO’s persistent event store for undelivered messages, such as `/opt/minio/events`.

MinIO stores undelivered events in the specified store while the AMQP service is offline and replays the stored events when connectivity resumes.

### Queue Limit

Optional

Environment Variable

MINIO_NOTIFY_AMQP_QUEUE_LIMIT[](#envvar.MINIO_NOTIFY_AMQP_QUEUE_LIMIT)

Configuration Setting

notify_amqp queue_limit[](#mc-conf.notify_amqp.queue_limit)

Specify the maximum limit for undelivered messages.
Defaults to `100000`.

### Comment

Optional

Environment Variable

MINIO_NOTIFY_AMQP_COMMENT[](#envvar.MINIO_NOTIFY_AMQP_COMMENT)

Configuration Setting

notify_amqp comment[](#mc-conf.notify_amqp.comment)

Specify a comment for the AMQP configuration.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).