---
title: "Redis feature sets"
source: "https://redis.io/docs/latest/develop/whats-new/redis-feature-sets/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/whats-new/redis-feature-sets"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis feature sets"
---
# Redis feature sets

Highlights of what's new for Redis feature sets

A Redis feature set includes a specific Redis database version along with the advanced capabilities and data structures provided by compatible module versions.

To use a new feature introduced in a later feature set, you must upgrade the corresponding components according to the following table.

Redis feature set
What's new

Feature set version: 8.8
See [[develop/whats-new/8-8|here]]

Feature set version: 8.6
See [[develop/whats-new/8-6|here]]

Feature set version: 8.4
See [[develop/whats-new/8-4|here]]

Feature set version: 8.2
See [[develop/whats-new/8-2|here]]

Feature set version: 8.0
See [[develop/whats-new/8-0|here]]

Feature set version: 7.4

Component versions:
[Redis 7.4](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisce/redisce-7.4-release-notes/)
[Search 2.10](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisearch/redisearch-2.10-release-notes/)
[JSON 2.8](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisjson/redisjson-2.8-release-notes/)
[Time series 1.12](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redistimeseries/redistimeseries-1.12-release-notes/)
[Bloom 2.8](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisbloom/redisbloom-2.8-release-notes/)
Hash: 
- [Expiration of individual hash fields](/docs/latest/develop/data-types/hashes/#field-expiration). 
 Streams: 
- To start reading from the last stream message, use [`XREAD`](/docs/latest/commands/xread/) with the new ID value `+`.
 Time series: 
Insertion-filter for close samples. 
 JSON: 
- A fix to not duplicate `AOF` commands multiple times in [`JSON.MSET`](/docs/latest/commands/json.mset/). 
 Probabilistic: 
- Returns an error if [`CMS.MERGE`](/docs/latest/commands/cms.merge/) results in an overflow or underflow. 
 Redis Search: 
- New `BFLOAT16` and `FLOAT16` vector data types, reducing memory consumed by vectors while preserving accuracy. 
- Support for indexing empty and missing values and enhanced developer experience for queries with exact matching capabilities. 
- You can match `TAG` fields without needing to escape special characters. 
- Expanded geospatial search with new `INTERSECT` and `DISJOINT` operators, improved reporting of the memory consumed by the index, and exposed full-text scoring in aggregation pipelines.

Feature set version: 7.2

Component versions:
[Redis 7.2](https://raw.githubusercontent.com/redis/redis/7.2/00-RELEASENOTES)
[Search 2.8](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisearch/redisearch-2.8-release-notes/)
[JSON 2.6](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisjson/redisjson-2.6-release-notes/)
[Time series 1.10](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redistimeseries/redistimeseries-1.10-release-notes/)
[Bloom 2.6](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisbloom/redisbloom-2.6-release-notes/)
[Gears 2.0](https://github.com/RedisGears/RedisGears/releases)
- Performance and resource utilization improvements, including significant memory and speed optimizations for lists, sets, and sorted sets.
JSON:
- New JSON commands: [`JSON.MERGE`](/docs/latest/commands/json.merge/) and [`JSON.MSET`](/docs/latest/commands/json.mset/).
Redis Search:
- [Geo polygon search](/docs/latest/commands/ft.search/#examples).
Compatibility changes:
- Redis 7.2 uses a new format (version 11) for RDB files, which is incompatible with older versions.
- Redis feature set 7.2 does not include [graph capabilities](https://redis.io/blog/redisgraph-eol/).

Feature set version: 6.2

Component versions:
[Redis 6.2](https://raw.githubusercontent.com/redis/redis/6.2/00-RELEASENOTES)
[Search 2.6](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisearch/redisearch-2.6-release-notes/)
[JSON 2.4](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisjson/redisjson-2.4-release-notes/)
[Time series 1.8](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redistimeseries/redistimeseries-1.8-release-notes/)
[Bloom 2.4](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisbloom/redisbloom-2.4-release-notes/)
[Graph 2.10](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisgraph/redisgraph-2.10-release-notes/)
Time series:
- Time series gap filling.
JSON:
- Improved JSON path parser.
Probabilistic:
- New probabilistic data structure t-digest.
Redis Search:
- Wildcard queries for `TEXT` and `TAG`.
- Suffix search.
- Multi-value indexing and queries.
Graph:
- New pathfinding algorithms for graphs.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)