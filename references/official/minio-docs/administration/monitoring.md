---
title: "Monitoring Bucket and Object Events — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/monitoring.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/monitoring"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Monitoring Bucket and Object Events — SILO (MinIO Object Storage)"
---
# Monitoring Bucket and Object Events

## Bucket Notifications

MinIO bucket notifications allow administrators to send notifications to supported external services on certain object or bucket events.
MinIO supports bucket and object-level S3 events similar to the
[Amazon S3 Event Notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/NotificationHowTo.html).

MinIO supports publishing bucket or object events to the following supported
targets on certain supported events.

[[administration/monitoring/publish-events-to-amqp#minio-bucket-notifications-publish-amqp|Publish Events to AMQP (RabbitMQ)]]

[[administration/monitoring/publish-events-to-mqtt#minio-bucket-notifications-publish-mqtt|Publish Events to MQTT]]

[[administration/monitoring/publish-events-to-nats#minio-bucket-notifications-publish-nats|Publish Events to NATS]]

[[administration/monitoring/publish-events-to-nsq#minio-bucket-notifications-publish-nsq|Publish Events to NSQ]]

[[administration/monitoring/publish-events-to-elasticsearch#minio-bucket-notifications-publish-elasticsearch|Publish Events to Elasticsearch]]

[[administration/monitoring/publish-events-to-kafka#minio-bucket-notifications-publish-kafka|Publish Events to Kafka]]

[[administration/monitoring/publish-events-to-mysql#minio-bucket-notifications-publish-mysql|Publish Events to MySQL]]

[[administration/monitoring/publish-events-to-postgresql#minio-bucket-notifications-publish-postgresql|Publish Events to PostgreSQL]]

[[administration/monitoring/publish-events-to-redis#minio-bucket-notifications-publish-redis|Publish Events to Redis]]

[[administration/monitoring/publish-events-to-webhook#minio-bucket-notifications-publish-webhook|Publish Events to Webhook]]

See [[administration/monitoring/bucket-notifications#minio-bucket-notifications|Bucket notifications]]
for more complete documentation on MinIO Bucket Notifications.

## Deployment Metrics

MinIO provides a Prometheus-compatible endpoint for supporting time-series querying of metrics.

## Server Logs

MinIO provides the following interfaces for remotely reading server logs:

The [[reference/minio-mc-admin/mc-admin-logs#command-mc.admin.logs|`mc admin logs`]] command returns the specified server’s console output.

MinIO supports pushing server logs to an HTTP webhook for further ingestion.
See [[operations/monitoring/minio-logging#minio-logging-publish-server-logs|Publish Server Logs to HTTP Webhook]] for more information.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).