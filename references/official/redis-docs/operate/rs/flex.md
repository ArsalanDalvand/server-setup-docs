---
title: "Flex databases"
source: "https://redis.io/docs/latest/operate/rs/flex/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/flex"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Flex databases"
---
# Flex databases

Extend Redis databases with flash storage for large-scale, cost-efficient deployments.

            Redis Software
          

            Redis Cloud
          

Flex extends your database capacity by combining RAM and flash (SSD) storage. This tiered architecture keeps frequently accessed (hot) data in RAM for sub-millisecond latency while storing less active (warm) data on flash to reduce costs and increase capacity. Flex frees RAM for hot data, leads to a higher RAM hit rate, and allows bigger datasets per node.

Flex databases work with your existing Redis applications and the Redis API without modification.

## How Flex works

### Automatic data tiering

Flex moves data between RAM and flash based on access patterns:

Frequently accessed data stays in high-speed RAM.
Less active data moves to cost-efficient flash storage.
Data accessed from flash promotes back to RAM automatically.

Redis uses an [[develop/reference/eviction#apx-lru|LRU (least recently used)]] eviction policy to manage data placement. When memory pressure increases, Flex identifies cold objects, transfers them to flash, and frees RAM for new or frequently accessed keys.

This process requires no application changes. Your existing Redis commands work across both storage tiers.

### Storage engine

Flex uses [Speedb](https://docs.speedb.io), a high-performance key-value storage engine optimized for flash drives:

Redis handles all data operations in memory.
Speedb manages the flash storage layer.

This design delivers predictable latency and throughput as datasets grow beyond RAM limits.

## Compatibility

Flex is compatible with the Redis API and supports all [Redis data types](/docs/latest/develop/data-types/), including JSON and probabilistic data structures (Bloom filters, Count-Min Sketch, Top-K).

The following features are not yet supported with Flex:

[Redis Search](/docs/latest/develop/ai/search-and-query/)
[Time series](/docs/latest/develop/data-types/timeseries/)
[Active-Active](/docs/latest/operate/rs/databases/active-active/)

## When to use Flex

Use Flex when you need to:

Run Redis at terabyte scale while maintaining high throughput and sub-10 ms latency
Power real-time feature stores for machine learning applications
Operate large distributed caches with elastic scaling and consistent performance under heavy load
Reduce infrastructure costs by combining high-speed RAM with cost-efficient flash storage

Note:
  Flex does not replace long-term data persistence. For workloads that require durability and recovery across restarts or failures, use Redis persistence features like [[operate/oss_and_stack/management/persistence#append-only-file|AOF (Append-Only File)]], [[operate/oss_and_stack/management/persistence#snapshotting|RDB snapshots]], or both. For more information, see [Database persistence](/docs/latest/operate/rs/databases/configure/database-persistence/).

## Flex and Auto Tiering

Flex replaces [[operate/rs/flex|Auto Tiering]] (formerly known as Redis on Flash). Redis Software selects the implementation based on your Redis version:

Redis database version
Flex
Auto Tiering

8.0 and later
â
â

7.4
â
â

7.2 and earlier
â
â

For Redis Software version 7.22.2-22 or earlier, see [[operate/rs/flex|Auto Tiering]].

### Differences between Flex and Auto Tiering

Key and value offloading

Auto Tiering offloads only values to flash while keys remain in RAM.
Flex offloads both keys and values, which increases dataset density per node and reduces RAM consumption. This change frees RAM for hot data, leads to a higher RAM hit rate, and allows bigger datasets per node.

RAM population strategy

Auto Tiering fills all available RAM before offloading data to flash. This maximizes hot-data performance but can cause non-linear performance changes at high utilization.
Flex uses utilization-aware RAM population. When database utilization is below 50%, Flex uses up to 50% of configured RAM for hot data. Above 50% utilization, Flex uses both RAM and flash proportionally, following the configured RAM-to-flash ratio. This provides a stable performance curve, consistent RAM hit-rate, and predictable throughput and latency.

Storage engine

Auto Tiering uses either RocksDB or Speedb as the storage engine.
Flex uses Speedb only.

## Next steps

[[operate/rs/flex/plan|Plan your deployment]]: Review hardware requirements, sizing guidelines, and limitations.
[[operate/rs/flex/get-started|Get started]]: Configure Flex on your cluster.
[[operate/rs/flex/scale|Scale your deployment]]: Learn scaling strategies and best practices.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page