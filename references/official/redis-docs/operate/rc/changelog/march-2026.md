---
title: "Redis Cloud changelog (March 2026)"
source: "https://redis.io/docs/latest/operate/rc/changelog/march-2026/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/changelog/march-2026"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud changelog (March 2026)"
---
# Redis Cloud changelog (March 2026)

New features, enhancements, and other changes added to Redis Cloud during March 2026.

            Redis Cloud
          

## New features

### Passwordless authentication for Redis Cloud Pro

Passwordless authentication is now available for Redis Cloud Pro databases on subscriptions that have [[operate/rc/security/database-security/block-public-endpoints|blocked the public endpoint]]. For more information, see [[operate/rc/security/database-security/block-public-endpoints#turn-on-passwordless-authentication-for-the-default-user|Turn on passwordless authentication for the default user]].

### Redis 8.4 on Redis Cloud Pro

Redis 8.4 is now available for [Redis Cloud Pro databases](/docs/latest/operate/rc/databases/create-database/create-essentials-database/) in select regions.

Redis 8.4 builds on the foundation of Redis 8.2 with significant enhancements to cluster operations, string manipulation, and stream processing capabilities. For more information on the changes in Redis 8.4, see [[develop/whats-new/8-4|What's new in Redis 8.4]] and review the Redis Open Source [8.4 release notes](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisce/redisos-8.4-release-notes/).

### Automatic database upgrades

All Redis Cloud databases running Redis 8.4 and later will be automatically upgraded to the next minor version upon release. For example, all Redis 8.4 databases will be upgraded to Redis 8.6 when it is available on Redis Cloud.

Redis Cloud Pro users can opt out of minor version auto-upgrades. See [Version management](/docs/latest/operate/rc/databases/version-management/) for more details.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page