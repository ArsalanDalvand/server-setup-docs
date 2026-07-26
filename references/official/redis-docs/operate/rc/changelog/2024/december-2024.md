---
title: "Redis Cloud changelog (December 2024)"
source: "https://redis.io/docs/latest/operate/rc/changelog/2024/december-2024/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/changelog/2024/december-2024"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud changelog (December 2024)"
---
# Redis Cloud changelog (December 2024)

New features, enhancements, and other changes added to Redis Cloud during December 2024.

            Redis Cloud
          

## New features

### Logs Viewer role

You can now add a user with the Logs Viewer role in the [[operate/rc/security/access-control/access-management|Access Management]] screen. Logs Viewers can only use the [[operate/rc/api|Redis Cloud API]] [`GET logs/`](https://api.redislabs.com/v1/swagger-ui/index.html#/Account/getAccountSystemLogs) endpoint.

See [[operate/rc/security/access-control/access-management#team-management-roles|Team Management roles]] to see an overview of user roles and their permissions.

### Redis Flex preview on Redis Cloud Essentials

Redis Flex is now available in Preview on Redis Cloud Essentials.

Redis Flex databases have a tiered solid state drive (SSD) and RAM architecture. Using SSDs instead of RAM significantly reduces infrastructure costs, which means developers can build applications that require large datasets using the same Redis API.

See [Create a Redis Flex database](/docs/latest/operate/rc/databases/create-database/create-flex-database/) for more info.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page