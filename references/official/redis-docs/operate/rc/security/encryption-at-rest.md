---
title: "Encryption at rest"
source: "https://redis.io/docs/latest/operate/rc/security/encryption-at-rest/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/security/encryption-at-rest"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Encryption at rest"
---
# Encryption at rest

Describes when data is encrypted at rest.

            Redis Cloud
          

Redis Cloud databases write their data to disk whenever [persistence](/docs/latest/operate/rc/databases/configuration/data-persistence/) is enabled.

Redis Cloud deployments are always encrypted at rest.

## Encryption at rest on AWS

Persistent data is written to encrypted EBS volumes. For more information, see [Amazon EBS encryption](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSEncryption.html).

When Auto Tiering is enabled, the flash memory data is written to encrypted NVMe SSD volumes. For more information, see [SSD instance store volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ssd-instance-store.html).

## Disk encryption on Google Cloud

All data written to disk on Google Cloud-based Redis Cloud deployments is encrypted by default. When deploying
a Redis Cloud database on Google Cloud, you don't need to take any actions to enable this encryption.

To learn more, see the [Google Cloud Default encryption at rest](https://cloud.google.com/security/encryption-at-rest).

## Disk encryption on Azure

All data written to disk on Azure-based Redis Cloud deployments is encrypted by default. When deploying
a Redis Cloud database on Azure, you don't need to take any actions to enable this encryption.

To learn more, see the [Azure Data Encryption at rest](https://docs.microsoft.com/en-us/azure/security/fundamentals/encryption-atrest).

## Continue learning with Redis University

See the [Redis Cloud security overview course](https://university.redis.io/course/0b1hkhejlitg3x) to learn more.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page