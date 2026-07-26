---
title: "Redis Cloud changelog (November 2023)"
source: "https://redis.io/docs/latest/operate/rc/changelog/2023/november-2023/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/changelog/2023/november-2023"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud changelog (November 2023)"
---
# Redis Cloud changelog (November 2023)

New features, enhancements, and other changes added to Redis Cloud during November 2023.

            Redis Cloud
          

## New features

### New Fixed plans

Redis is updating our [Fixed subscriptions](/docs/latest/operate/rc/databases/create-database/create-essentials-database/) on Redis Cloud. These plans offer increased capacity at lower prices compared to legacy plans.

See [Fixed plan subscription tiers](/docs/latest/operate/rc/databases/create-database/create-essentials-database/#fixed-plans) for an updated list of all Fixed plans.

### Redis 7.2 Fixed region support

You can now use Redis 7.2 on [Fixed subscriptions](/docs/latest/operate/rc/databases/create-database/create-essentials-database/) in `us-east-1` on Amazon Web Services. New Fixed subscriptions created in `us-east-1` will use Redis 7.2, and existing subscriptions will continue to use Redis 6.2.

Redis 7.2 introduces several changes to existing Redis commands; see the [[operate/rc/changelog/2023/june-2023#redis-72-breaking-changes|list of breaking changes]] published in June's changelog for more details.

## Deprecations

### Heroku add-on free database limit

Heroku add-ons for [Redis Cloud](https://elements.heroku.com/addons/rediscloud) and [Memcached Cloud](https://elements.heroku.com/addons/memcachedcloud) are limited to one free database per Heroku account. As of November 1st, users will not be able to add a new free database if they already have one or more free databases. There will be no impact to existing free and paid databases.

Additionally, Heroku Review and CI Apps will provision a 100 MB database for the [Redis Cloud](https://elements.heroku.com/addons/rediscloud) and [Memcached Cloud](https://elements.heroku.com/addons/memcachedcloud) add-on selections.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page