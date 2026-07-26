---
title: "Multi-key operations"
source: "https://redis.io/docs/latest/develop/using-commands/multi-key-operations/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/using-commands/multi-key-operations"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Multi-key operations"
---
# Multi-key operations

Multi-key command behavior across Redis configurations and clustering setups

Multi-key operations in Redis allow you to work with multiple keys in a single command, but their behavior varies significantly depending on your Redis configuration and clustering setup. This page provides a quick reference for developers working with multi-key operations across different Redis configurations.

## Configurations

Redis supports five distinct configurations, each with different multi-key command behaviors:

ROS/RS clustering disabled - Single Redis instance
ROS, clustering enabled - Redis Open Source cluster
RS, clustering enabled, OSS cluster API enabled - Redis Software with ROS cluster compatibility
RS, clustering enabled, OSS cluster API disabled - Redis Software proprietary clustering
RS, Active-Active - Redis Software Active-Active (considered clustered even with a single shard)

ROS stands for Redis Open Source and RS stands for Redis Software.

## Command behaviors

For each configuration, commands exhibit one of three behaviors:

single-slot: Commands must operate on keys within the same hash slot
cross-slot (all shards): Commands can operate across all shards in the cluster
cross-slot (within a single shard): Commands can operate across slots but only within a single shard

## Read-only commands

Behavior
Commands

ROS/RS clustering disabled:
â the whole DB (single shard)

ROS clustering enabled, RS clustering enabled (OSS cluster API enabled):
â the current shard

RS clustering enabled (OSS cluster API disabled):
â all shards
DBSIZE, KEYS, SCAN

ROS/RS clustering disabled:
â cross-slot

ROS clustering enabled, RS clustering enabled (OSS cluster API enabled):
â single-slot

RS clustering enabled (OSS cluster API disabled):
â cross-slot (all shards)
EXISTS, MGET

ROS/RS clustering disabled:
â cross-slot

ROS clustering enabled, RS clustering enabled (OSS cluster API enabled), RS clustering enabled (OSS cluster API disabled):
â single-slot
PFCOUNT, SDIFF, SINTER, SINTERCARD, SUNION, WATCH, XREAD, XREADGROUP, ZDIFF, ZINTER, ZINTERCARD, ZUNION

ROS/RS clustering disabled:
â cross-slot

ROS clustering enabled, RS clustering enabled (OSS cluster API enabled), RS clustering enabled (OSS cluster API disabled):
â single-shard
JSON.MGET

Users won't get a CROSSSLOT error. However, when clustering is enabled, and not all specified keys are in the same slot, users will get partial results for all the slots on the current shard.

ROS/RS clustering disabled:
â cross-slot (all shards)

ROS clustering enabled, RS clustering enabled (OSS cluster API enabled), RS clustering enabled (OSS cluster API disabled):
â cross-slot (all shards), cannot be part of a transaction
TS.MGET, TS.MRANGE, TS.MREVRANGE, TS.QUERYINDEX

## Read-write commands

Behavior
Commands

ROS/RS clustering disabled:
â the whole DB (single shard)

ROS clustering enabled, RS clustering enabled (OSS cluster API enabled):
â the current shard

RS clustering enabled (OSS cluster API disabled):
â all shards
FLUSHALL, FLUSHDB

ROS/RS clustering disabled:
â cross-slot

ROS clustering enabled, RS clustering enabled (OSS cluster API enabled):
â single-slot

RS clustering enabled (OSS cluster API disabled):
â cross-slot (all shards)
DEL, MSET, TOUCH, UNLINK

Note: on Active-Active, DEL, MSET, and UNLINK are single-slot

ROS/RS clustering disabled:
â cross-slot

ROS clustering enabled, RS clustering enabled (OSS cluster API enabled), RS clustering enabled (OSS cluster API disabled):
â single-slot
BITOP, BLMOVE, BLMPOP, BLPOP, BRPOP, BRPOPLPUSH, BZMPOP, BZPOPMAX, BZPOPMIN, CMS.MERGE, COPY, GEORADIUS or GEORADIUSBYMEMBER (with STORE or STOREDIST), GEOSEARCHSTORE, JSON.MSET, LMOVE, LMPOP, MSETNX, PFMERGE, RENAME, RENAMENX, RPOPLPUSH, SDIFFSTORE, SINTERSTORE, SMOVE, SUNIONSTORE, TDIGEST.MERGE, TS.MADD, ZDIFFSTORE, ZINTERSTORE, ZMPOP, ZRANGESTORE, ZUNIONSTORE

ROS/RS clustering disabled:
â cross-slot

ROS clustering enabled, RS clustering enabled (OSS cluster API enabled), RS clustering enabled (OSS cluster API disabled):
â single-shard
TS.CREATERULE, TS.DELETERULE

Users won't get a CROSSSLOT error. However, when clustering is enabled and the two specified keys are not in the same slot, users will get `(error) ERR TSDB: the key does not exist`.

## Pipelines, transactions, and scripts

Behavior
Operations

ROS/RS clustering disabled:
â cross-slot
ROS clustering enabled, RS clustering enabled (OSS cluster API enabled):
â single-slot
RS clustering enabled (OSS cluster API disabled):
â cross-slot (all shards)
Pipelines

ROS/RS clustering disabled:
â cross-slot
ROS clustering enabled, RS clustering enabled (OSS cluster API enabled), RS clustering enabled (OSS cluster API disabled):
â single-slot
Keys in a `MULTI/EXEC` transaction
Keys in a Lua script executed using EVAL or EVALSHA

## Examples by Configuration

### Single Instance (No Clustering)

In a single Redis instance, all multi-key operations work without restrictions:

```redis
# Pipeline operations work across any keys
PIPELINE
SET user:1 "Alice"
SET product:100 "Widget"
GET user:1
GET product:100
EXEC

# Transactions work with any keys
MULTI
SET counter:a 1
SET counter:b 2
INCR counter:a
INCR counter:b
EXEC
```

### Clustered Environments

In clustered setups, you need to consider slot distribution:

```redis
# This may fail if keys are in different slots
MSET user:1 "Alice" user:2 "Bob"

# Use hash tags to ensure same slot
MSET {users}:1 "Alice" {users}:2 "Bob"

# Check which slot a key belongs to
CLUSTER KEYSLOT user:1
CLUSTER KEYSLOT {users}:1
```

### Active-Active Databases

Active-Active databases have additional restrictions for write operations:

```redis
# Read operations can work across slots
MGET user:1 user:2 product:100

# Write operations must be in same slot
MSET {data}:user:1 "Alice" {data}:user:2 "Bob"
```

## Troubleshooting Multi-Key Operations

### Common Error Messages

CROSSSLOT: Keys in request don't hash to the same slot
MOVED: Key has moved to a different node (during resharding)
TRYAGAIN: Operation temporarily unavailable (during migration)

### Solutions

Use hash tags to group related keys
Redesign data model to minimize cross-slot operations
Check cluster state during errors
Implement retry logic for temporary failures

## Performance Considerations

Single-slot operations are fastest as they don't require coordination
Cross-slot operations may have higher latency due to internal routing
Pattern commands (KEYS, FLUSHALL) scan all shards and can be expensive
Module operations may have optimized cross-slot implementations

Choose your Redis configuration and design your data model based on your multi-key operation requirements.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page