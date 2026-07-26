---
title: "Redis Cloud changelog (October 2025)"
source: "https://redis.io/docs/latest/operate/rc/changelog/october-2025/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/changelog/october-2025"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud changelog (October 2025)"
---
# Redis Cloud changelog (October 2025)

New features, enhancements, and other changes added to Redis Cloud during October 2025.

            Redis Cloud
          

## New features

### Data Integration

Redis Cloud now supports [[operate/rc/rdi|Redis Data Integration (RDI)]] to create data pipelines that ingest data from a supported primary database to Redis.

Using a data pipeline lets you have a cache that is always ready for queries. RDI Data pipelines ensure that any changes made to your primary database are captured in your Redis cache within a few seconds, preventing cache misses and stale data within the cache.

See [[operate/rc/rdi|Data Integration]] to learn how to set up data pipelines with Redis Cloud.

## Security fixes

Redis Cloud has already been updated with a patches for the following vulnerabilities:

(CVE-2025-49844) A Lua script may lead to remote code execution
(CVE-2025-46817) A Lua script may lead to integer overflow and potential RCE
(CVE-2025-46818) A Lua script can be executed in the context of another user
(CVE-2025-46819) LUA out-of-bound read

No further action is required at this time.

For more information, see the [Redis blog](https://redis.io/blog/security-advisory-cve-2025-49844/) and the [Redis release notes](https://github.com/redis/redis/releases).

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page