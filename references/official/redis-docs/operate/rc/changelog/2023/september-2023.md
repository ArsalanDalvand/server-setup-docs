---
title: "Redis Cloud changelog (September 2023)"
source: "https://redis.io/docs/latest/operate/rc/changelog/2023/september-2023/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/changelog/2023/september-2023"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud changelog (September 2023)"
---
# Redis Cloud changelog (September 2023)

New features, enhancements, and other changes added to Redis Cloud during September 2023.

            Redis Cloud
          

## New features

### RESP protocol selection

For all databases using Redis 7.2, you can now choose between the RESP2 and RESP3 protocols when you [create a database](/docs/latest/operate/rc/databases/create-database/). For more information about the different RESP versions, see the [[develop/reference/protocol-spec#resp-versions|Redis serialization protocol specification]].

### Opt-in to Redis 7.2

Select Opt-in to 7.2 to request to upgrade your subscription and databases to Redis 7.2 (Redis 6.2 and earlier only).

[](/docs/latest/images/rc/button-opt-in-to-72.png)

The upgrade will start one week from your request, according to [[operate/rc/subscriptions/maintenance/set-maintenance-windows|maintenance windows]].

Redis 7.2 introduces several changes to existing Redis commands; see the list of [[operate/rc/changelog/2023/june-2023#redis-72-breaking-changes|breaking changes]] for more details.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page