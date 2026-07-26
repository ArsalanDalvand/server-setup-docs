---
title: "Install RedisGears for Redis Data Integration"
source: "https://redis.io/docs/latest/integrate/write-behind/installation/install-redis-gears/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/write-behind/installation/install-redis-gears"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Install RedisGears for Redis Data Integration"
---
# Install RedisGears for Redis Data Integration

Install and set up RedisGears for a Write-behind deployment

Write-behind requires that [RedisGears](https://redis.com/modules/redis-gears) module with the [Python plugin](https://docs.redis.com/latest/modules/redisgears/python/) is installed on the Redis Enterprise cluster.

The Python plugin can be installed explicitly or alongside with the [JVM plugin](https://docs.redis.com/latest/modules/redisgears/jvm/) if the latter is needed on the cluster for other purposes.

Use the [[integrate/write-behind/reference/cli/redis-di-create|`redis-di create`]] command in Write-behind CLI to install RedisGears.

## Download RedisGears

Download RedisGears based on the Linux distribution of where Redis Enterprise is installed.

### Ubuntu 20.04

```bash
curl -s --tlsv1.3 https://redismodules.s3.amazonaws.com/redisgears/redisgears.Linux-ubuntu20.04-x86_64.1.2.6-withdeps.zip -o /tmp/redis-gears.zip
```

### Ubuntu 18.04

```bash
curl -s --tlsv1.3 https://redismodules.s3.amazonaws.com/redisgears/redisgears.Linux-ubuntu18.04-x86_64.1.2.6-withdeps.zip -o /tmp/redis-gears.zip
```

### RHEL8

```bash
curl -s https://redismodules.s3.amazonaws.com/redisgears/redisgears.Linux-rhel8-x86_64.1.2.6-withdeps.zip -o /tmp/redis-gears.zip
```

### RHEL7

```bash
curl -s https://redismodules.s3.amazonaws.com/redisgears/redisgears.Linux-rhel7-x86_64.1.2.6-withdeps.zip -o /tmp/redis-gears.zip
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page