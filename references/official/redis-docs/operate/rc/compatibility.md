---
title: "Redis Cloud compatibility with Redis Open Source"
source: "https://redis.io/docs/latest/operate/rc/compatibility/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/compatibility"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud compatibility with Redis Open Source"
---
# Redis Cloud compatibility with Redis Open Source

Redis Cloud compatibility with Redis Open Source.

            Redis Cloud
          

Both [[operate/rs|Redis Software]] and Redis Cloud are compatible with Redis Open Source.

Redis contributes extensively to the Redis project and uses it inside of Redis Software and Redis Cloud. As a rule, we adhere to the project's specifications and update both productsÂ with the latest version of Redis.

## Redis commands

See [Compatibility with Redis commands](/docs/latest/operate/rs/references/compatibility/commands/) to learn which Redis commands are compatible with Redis Software and Redis Cloud.

## Configuration settings

[Compatibility with Redis configuration settings](/docs/latest/operate/rs/references/compatibility/config-settings/) lists the Redis configuration settings supported by Redis Software and Redis Cloud.

## Redis clients

You can use any standard [[develop/clients|Redis client]] with Redis Software and Redis Cloud.

## RESP compatibility

Redis Software and Redis Cloud support RESP2 and RESP3. In Redis Cloud, you can choose between RESP2 and RESP3 when you [create a database](/docs/latest/operate/rc/databases/create-database/) and you can change it when you [edit a database](/docs/latest/operate/rc/databases/view-edit-database/). For more information about the different RESP versions, see the [[develop/reference/protocol-spec#resp-versions|Redis serialization protocol specification]].

## Client-side caching compatibility

Redis Software and Redis Cloud support [[develop/clients/client-side-caching|client-side caching]] for databases with Redis versions 7.4 or later. See [Client-side caching compatibility with Redis Software and Redis Cloud](/docs/latest/operate/rs/references/compatibility/client-side-caching/) for more information about compatibility.

## Compatibility with Redis Cluster API

Redis Cloud supportsÂ [Redis Cluster API](/docs/latest/operate/rc/databases/configuration/clustering/#oss-cluster-api) on Redis Cloud Pro if it is enabled for a database. Review [[operate/rs/clusters/optimize/oss-cluster-api|Redis Cluster API architecture]] to determine if you should enable this feature for your database.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page