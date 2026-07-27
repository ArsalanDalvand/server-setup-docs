---
title: "mc event — SILO (MinIO Object Storage)"
source: "https://silo.pigsty.io/reference/minio-mc/mc-event.html"
source_format: official-html-to-markdown
language: "en"
doc_slug: "reference/minio-mc/mc-event"
category: "reference"
retrieved_at: "2026-07-26T19:44:53+00:00"
tags:
  - minio
  - s3-storage
  - object-store
  - official-documentation
aliases:
  - "mc event — SILO (MinIO Object Storage)"
---
# mc event

## Description

The [`mc event`](#command-mc.event) command supports adding, removing, and listing bucket event notifications.

MinIO automatically sends triggered events to the configured notification targets.
MinIO supports notification targets like AMQP (RabbitMQ), Redis, ElasticSearch, NATS and PostgreSQL.
See [[administration/monitoring/bucket-notifications#minio-bucket-notifications|MinIO Bucket Notifications]] for more information.

## Subcommands

[`mc event`](#command-mc.event) includes the following subcommands:

Subcommand

Description

[[reference/minio-mc/mc-event-add#command-mc.event.add|`add`]]

The [[reference/minio-mc/mc-event-add#command-mc.event.add|`mc event add`]] command adds event notification triggers to a bucket.

[[reference/minio-mc/mc-event-list#command-mc.event.ls|`ls`]]

The [[reference/minio-mc/mc-event-list#command-mc.event.ls|`mc event ls`]] command lists all event notification triggers for a
bucket.

[[reference/minio-mc/mc-event-remove#command-mc.event.rm|`rm`]]

The [[reference/minio-mc/mc-event-remove#command-mc.event.rm|`mc event rm`]] command removes an event notification trigger from a bucket.

   This work is licensed under a
   [Creative Commons Attribution 4.0 International License.](http://creativecommons.org/licenses/by/4.0/)
   2020-Present
   [](http://creativecommons.org/licenses/by/4.0/)

Your privacy is important to us:
            We use cookies in order to give you a better experience. If you wish so, you can always review our 
            [Privacy Policy](https://min.io/privacy-policy).