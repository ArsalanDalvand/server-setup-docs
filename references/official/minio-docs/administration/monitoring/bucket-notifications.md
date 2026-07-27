---
title: "Bucket notifications — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/administration/monitoring/bucket-notifications.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "administration/monitoring/bucket-notifications"
category: "administration"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "Bucket notifications — SILO (MinIO Object Storage)"
---
# Bucket notifications

MinIO bucket notifications allow administrators to send notifications to supported external services on certain object or bucket events.
MinIO supports bucket and object-level S3 events similar to the
[Amazon S3 Event Notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/NotificationHowTo.html).

## Supported notification targets

MinIO supports publishing event notifications to the following targets:

Target

Description

AMQP (RabbitMQ)

Publish notifications to an AMQP service such as
[RabbitMQ](https://www.rabbitmq.com).

See [[administration/monitoring/publish-events-to-amqp#minio-bucket-notifications-publish-amqp|Publish Events to AMQP (RabbitMQ)]] for a tutorial.

MQTT

Publish notifications to an [MQTT](https://www.mqtt.org/) service.

See [[administration/monitoring/publish-events-to-mqtt#minio-bucket-notifications-publish-mqtt|Publish Events to MQTT]] for a tutorial.

NATS

Publish notifications to a [NATS](https://nats.io/) service.

See [[administration/monitoring/publish-events-to-nats#minio-bucket-notifications-publish-nats|Publish Events to NATS]] for a tutorial.

NSQ

Publish notifications to a [NSQ](https://nsq.io/) service.

See [[administration/monitoring/publish-events-to-nsq#minio-bucket-notifications-publish-nsq|Publish Events to NSQ]] for a tutorial

Elasticsearch

Publish notifications to a [Elasticsearch](https://www.elastic.co/) service.

See [[administration/monitoring/publish-events-to-elasticsearch#minio-bucket-notifications-publish-elasticsearch|Publish Events to Elasticsearch]] for a tutorial.

Kafka

Publish notifications to a [Kafka](https://kafka.apache.org/) service.

See [[administration/monitoring/publish-events-to-kafka#minio-bucket-notifications-publish-kafka|Publish Events to Kafka]] for a tutorial.

MySQL

Publish notifications to a [MySQL](https://www.mysql.com/) service.

See [[administration/monitoring/publish-events-to-mysql#minio-bucket-notifications-publish-mysql|Publish Events to MySQL]] for a tutorial.

PostgreSQL

Publish notifications to a [PostgreSQL](https://www.postgresql.org/) service.

See [[administration/monitoring/publish-events-to-postgresql#minio-bucket-notifications-publish-postgresql|Publish Events to PostgreSQL]] for a tutorial.

Redis

Publish notifications to a [Redis](https://redis.io/) service.

See [[administration/monitoring/publish-events-to-redis#minio-bucket-notifications-publish-redis|Publish Events to Redis]] for a tutorial.

webhook

Publish notifications to a [Webhook](https://en.wikipedia.org/wiki/Webhook) service.

See [[administration/monitoring/publish-events-to-webhook#minio-bucket-notifications-publish-webhook|Publish Events to Webhook]] for a tutorial.

## Asynchronous vs synchronous bucket notifications

New in version RELEASE.2023-06-23T20-26-00Z: MinIO supports either asynchronous (default) or synchronous bucket notifications for all remote targets.

With asynchronous delivery, MinIO fires the event at the configured remote and does not wait for a response before continuing to the next event.
Asynchronous bucket notification prioritizes sending events with the risk of some events being lost if the remote target has a transient issue during transit or processing.

With synchronous delivery, MinIO fires the event at the configured remote and then waits for the remote to confirm a successful receipt before continuing to the next event.
Synchronous bucket notification prioritizes delivery of events with the risk of a slower event-send rate and queue fill.

To enable synchronous bucket notifications for all configured remote targets, use either of the following settings:

Set the [[reference/minio-server/settings/notifications#envvar.MINIO_API_SYNC_EVENTS|`MINIO_API_SYNC_EVENTS`]] environment variable to `on` and restart the MinIO deployment.

Set the [[reference/minio-server/settings/notifications#mc-conf.api.sync_events|`api.sync_events`]] configuration setting to `on` and restart the MinIO deployment.

Note

For synchronous and asynchronous events, MinIO maintains a per-remote queue where it stores unsent and pending events.
The queue limit defaults to `100000`.

MinIO discards new events when the queue is full.

You can increase the queue size as necessary to better accommodate the rate of event send and processing of the MinIO deployment and remote target.
Use the `QUEUE_LIMIT` environment variable or configuration setting for your notification method to modify this limit.

For asynchronous events, MinIO allows a maximum of `50000` concurrent `send` calls.

## Supported S3 event types

MinIO bucket notifications are compatible with [Amazon S3 Event Notifications](https://docs.aws.amazon.com/AmazonS3/latest/userguide/NotificationHowTo.html).
This section lists all supported events.

### Object events

MinIO supports triggering notifications on the following S3 object events:

s3:ObjectAccessed:Get[](#data.s3-ObjectAccessed-Get)

s3:ObjectAccessed:GetLegalHold[](#data.s3-ObjectAccessed-GetLegalHold)

s3:ObjectAccessed:GetRetention[](#data.s3-ObjectAccessed-GetRetention)

s3:ObjectAccessed:Head[](#data.s3-ObjectAccessed-Head)

s3:ObjectCreated:CompleteMultipartUpload[](#data.s3-ObjectCreated-CompleteMultipartUpload)

s3:ObjectCreated:Copy[](#data.s3-ObjectCreated-Copy)

s3:ObjectCreated:DeleteTagging[](#data.s3-ObjectCreated-DeleteTagging)

s3:ObjectCreated:Post[](#data.s3-ObjectCreated-Post)

s3:ObjectCreated:Put[](#data.s3-ObjectCreated-Put)

s3:ObjectCreated:PutLegalHold[](#data.s3-ObjectCreated-PutLegalHold)

s3:ObjectCreated:PutRetention[](#data.s3-ObjectCreated-PutRetention)

s3:ObjectCreated:PutTagging[](#data.s3-ObjectCreated-PutTagging)

s3:ObjectRemoved:Delete[](#data.s3-ObjectRemoved-Delete)

s3:ObjectRemoved:DeleteMarkerCreated[](#data.s3-ObjectRemoved-DeleteMarkerCreated)

Specify the wildcard `*` character to select all events related to a prefix:

s3:ObjectAccessed:*[](#data.s3-ObjectAccessed)

Selects all `s3:ObjectAccessed`-prefixed events.

s3:ObjectCreated:*[](#data.s3-ObjectCreated)

Selects all `s3:ObjectCreated`-prefixed events.

s3:ObjectRemoved:*[](#data.s3-ObjectRemoved)

Selects all `s3:ObjectRemoved`-prefixed events.

### Replication events

MinIO supports triggering notifications on the following S3 replication events:

s3:Replication:OperationCompletedReplication[](#data.s3-Replication-OperationCompletedReplication)

s3:Replication:OperationFailedReplication[](#data.s3-Replication-OperationFailedReplication)

s3:Replication:OperationMissedThreshold[](#data.s3-Replication-OperationMissedThreshold)

s3:Replication:OperationNotTracked[](#data.s3-Replication-OperationNotTracked)

s3:Replication:OperationReplicatedAfterThreshold[](#data.s3-Replication-OperationReplicatedAfterThreshold)

Specify the wildcard `*` character to select all `s3:Replication` events:

s3:Replication:*[](#data.s3-Replication)

### ILM transition events

MinIO supports triggering notifications on the following S3 ILM transition events:

s3:ObjectRestore:Post[](#data.s3-ObjectRestore-Post)

s3:ObjectRestore:Completed[](#data.s3-ObjectRestore-Completed)

s3:ObjectTransition:Failed[](#data.s3-ObjectTransition-Failed)

s3:ObjectTransition:Complete[](#data.s3-ObjectTransition-Complete)

Specify the wildcard `*` character to select all events related to a prefix:

s3:ObjectTransition:*[](#data.s3-ObjectTransition)

Selects all `s3:ObjectTransition`-prefixed events.

s3:ObjectRestore:*[](#data.s3-ObjectRestore)

Selects all `s3:ObjectRestore`-prefixed events.

### Scanner events

MinIO supports triggering notifications on the following S3 [[operations/concepts/scanner#minio-concepts-scanner|scanner]] transition events:

s3:Scanner:ManyVersions[](#data.s3-Scanner-ManyVersions)

[[operations/concepts/scanner#minio-concepts-scanner|Scanner]] finds objects with more than 1,000 versions.

s3:Scanner:BigPrefix[](#data.s3-Scanner-BigPrefix)

[[operations/concepts/scanner#minio-concepts-scanner|Scanner]] finds prefixes with more than 50,000 sub-folders.

### Global events

MinIO supports triggering notifications on the following global events.
You can only listen to these events through the [ListenNotification](https://silo.pigsty.io/developers/go/API.html#listennotification-context-context-context-prefix-suffix-string-events-string-chan-notification-info) API:

s3:BucketCreated[](#data.s3-BucketCreated)

s3:BucketRemoved[](#data.s3-BucketRemoved)

## Payload schema

All notification payloads use the same overall schema.
Depending on the type of notification, some fields may be omitted or have null values.

```
{
    "eventVersion": "string",
    "eventSource": "string",
    "awsRegion": "string",
    "eventTime": "string",
    "eventName": "string",
    "userIdentity": {
        "principalId": "string"
    },
    "requestParameters": {
        "key": "value"
    },
    "responseElements": {
        "key": "value"
    },
    "s3": {
        "s3SchemaVersion": "string",
        "configurationId": "string",
        "bucket": {
            "name": "string",
            "ownerIdentity": {
                "principalId": "string"
            },
            "arn": "string"
        },
        "object": {
            "key": "string",
            "size": 10000,
            "eTag": "string",
            "contentType": "string",
            "userMetadata": {
                "key": "string"
            },
            "versionId": "string",
            "sequencer": "string"
        }
    },
    "source": {
        "host": "string",
        "port": "string",
        "userAgent": "string"
    }
}
```

### Example

The following example is a notification for an `s3:ObjectCreated:Put` event:

```
{
  "EventName": "s3:ObjectCreated:Put",
  "Key": "test-bucket/image.jpg",
  "Records": [
    {
      "eventVersion": "2.0",
      "eventSource": "minio:s3",
      "awsRegion": "",
      "eventTime": "2025-02-06T01:04:31.998Z",
      "eventName": "s3:ObjectCreated:Put",
      "userIdentity": {
        "principalId": "access_key"
      },
      "requestParameters": {
        "principalId": "access_key",
        "region": "",
        "sourceIPAddress": "192.168.1.10"
      },
      "responseElements": {
        "x-amz-id-2": "dd9025bab4ad464b049177c95eb6ebf374d3b3fd1af9251148b658df7ac2e3e8",
        "x-amz-request-id": "182178E8B36AC9DF",
        "x-minio-deployment-id": "2369dcb4-348b-4d30-8fc9-61ab089ba4bc",
        "x-minio-origin-endpoint": "https://minio.test.svc.cluster.local"
      },
      "s3": {
        "s3SchemaVersion": "1.0",
        "configurationId": "Config",
        "bucket": {
          "name": "test-bucket",
          "ownerIdentity": {
            "principalId": "access_key"
          },
          "arn": "arn:aws:s3:::test-bucket"
        },
        "object": {
          "key": "image.jpg",
          "size": 84452,
          "eTag": "eb52f8e46f60a27a8a1a704e25757f30",
          "contentType": "image/jpeg",
          "userMetadata": {
            "content-type": "image/jpeg"
          },
          "sequencer": "182178E8B3728CAC"
        }
      },
      "source": {
        "host": "192.168.1.10",
        "port": "",
        "userAgent": "MinIO (linux; amd64) minio-go/v7.0.83"
      }
    }
  ]
}
```

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).