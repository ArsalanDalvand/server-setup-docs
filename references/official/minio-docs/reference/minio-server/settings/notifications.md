---
title: "Bucket Notifications Settings — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-server/settings/notifications.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-server/settings/notifications"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Bucket Notifications Settings — SILO (MinIO Object Storage)"
---
# Bucket Notifications Settings

This page covers settings that control behavior related to [[administration/monitoring/bucket-notifications#minio-bucket-notifications|MinIO bucket notifications]].

You can establish or modify settings by defining:

an environment variable on the host system prior to starting or restarting the MinIO Server.
Refer to your operating system’s documentation for how to define an environment variable.

a configuration setting using [[reference/minio-mc-admin/mc-admin-config#mc.admin.config.set|`mc admin config set`]].

If you define both an environment variable and the similar configuration setting, MinIO uses the environment variable value.

Some settings have only an environment variable or a configuration setting, but not both.

Important

Each configuration setting controls fundamental MinIO behavior and functionality.
MinIO strongly recommends testing configuration changes in a lower environment, such as DEV or QA, before applying to production.

## Sync Events

Optional

Environment Variable

MINIO_API_SYNC_EVENTS[](#envvar.MINIO_API_SYNC_EVENTS)

Configuration Setting

api sync_events[](#mc-conf.api.sync_events)

Enables synchronous [[administration/monitoring/bucket-notifications#minio-bucket-notifications|bucket notifications]].

Specify `on` to direct MinIO to wait until the remote target returns success on receipt of an event before processing further events.

Defaults to `off`, or asynchronous bucket notifications where MinIO does not wait for the remote target to return success on receipt of an event.

## Supported Notification Targets

Notifications require a target to receive the events.
MinIO supports a variety of possible targets.
Settings for each target type have their own pages.
Select the appropriate link below for the type of target you use for notifications.

[[reference/minio-server/settings/notifications/amqp#minio-server-envvar-bucket-notification-amqp|AMQP Notification Settings]]

[[reference/minio-server/settings/notifications/elasticsearch#minio-server-envvar-bucket-notification-elasticsearch|Elasticsearch Notification Settings]]

[[reference/minio-server/settings/notifications/kafka#minio-server-envvar-bucket-notification-kafka|Kafka Notification Settings]]

[[reference/minio-server/settings/notifications/mqtt#minio-server-envvar-bucket-notification-mqtt|MQTT Notification Settings]]

[[reference/minio-server/settings/notifications/mysql#minio-server-envvar-bucket-notification-mysql|MySQL Notification Settings]]

[[reference/minio-server/settings/notifications/nats#minio-server-envvar-bucket-notification-nats|NATS Notification Settings]]

[[reference/minio-server/settings/notifications/nsq#minio-server-envvar-bucket-notification-nsq|NSQ Notification Settings]]

[[reference/minio-server/settings/notifications/postgresql#minio-server-envvar-bucket-notification-postgresql|PostgreSQL Notification Settings]]

[[reference/minio-server/settings/notifications/redis#minio-server-envvar-bucket-notification-redis|Redis Notification Settings]]

[[reference/minio-server/settings/notifications/webhook-service#minio-server-envvar-bucket-notification-webhook|Webhook Service Notification Settings]]

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).