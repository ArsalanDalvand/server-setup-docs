---
title: "Disk sizing for heavy write scenarios"
source: "https://redis.io/docs/latest/operate/rs/clusters/optimize/disk-sizing-heavy-write-scenarios/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/clusters/optimize/disk-sizing-heavy-write-scenarios"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Disk sizing for heavy write scenarios"
---
# Disk sizing for heavy write scenarios

Sizing considerations for persistent disk space for heavy throughput databases.

            Redis Software
          

In extreme write scenarios when append-only files (AOF) are enabled, the AOF rewrite process can require considerably more disk space for [database persistence](/docs/latest/operate/rs/databases/configure/database-persistence/).

For disk size requirements for standard usage, see [[operate/rs/installing-upgrading/install/plan-deployment/hardware-requirements|Hardware requirements]].

## Estimate required disk space

To estimate the required persistent disk space for AOF rewrite purposes in extreme write scenarios, use the following formula:

X (1 + 3Y + YÂ²)

Where:

X = size of each shard in GB
Y = number of shards

## Examples

The following examples show how to calculate the persistent disk space required for heavy write scenarios for different database configurations, where:

Database size is the memory limit configured for the database.

Number of shards is the total number of shards (primary shards + replica shards).

### Example 1

Database size = 10 GB
Number of shards = 4

Calculate the shard size:

```sh
Shard size = database size / number of shards
           = 10 GB / 4 shards
           = 2.5 GB per shard
```

Use the formula to calculate the required persistent disk space:

```sh
Disk space = X (1 + 3Y + YÂ²)
           = 2.5 (1 + 3 Ã 4 + 4Â²)
           = 2.5 (1 + 12 + 16)
           = 2.5 Ã 29
           = 72.5 GB
```

Round up to 73 GB of required disk space.

### Example 2

Database size = 10 GB
Number of shards = 16

Calculate the shard size:

```sh
Shard size = database size / number of shards
           = 10 GB / 16 shards
           = 0.625 GB per shard
```

Use the formula to calculate the required persistent disk space:

```sh
Disk space = X (1 + 3Y + YÂ²)
           = 0.625 (1 + 3 Ã 16 + 16Â²)
           = 0.625 (1 + 48 + 256)
           = 0.625 Ã 305
           = 190.625 GB
```

Round up to 191 GB of required disk space.

### Example 3

Database size = 40 GB
Number of shards = 5

Calculate the shard size:

```sh
Shard size = database size / number of shards
           = 40 GB / 5 shards
           = 8 GB per shard
```

Use the formula to calculate the required persistent disk space:

```sh
Disk space = X (1 + 3Y + YÂ²)
           = 8 (1 + 3 Ã 5 + 5Â²)
           = 8 (1 + 15 + 25)
           = 8 Ã 41
           = 328 GB
```

Required disk space: 328 GB.

### Example 4

Database size = 40 GB
Number of shards = 15

Calculate the shard size:

```sh
Shard size = database size / number of shards
           = 40 GB / 15 shards
           = 2.67 GB per shard
```

Use the formula to calculate the required persistent disk space:

```sh
Disk space = X (1 + 3Y + YÂ²)
           = 2.67 (1 + 3 Ã 15 + 15Â²)
           = 2.67 (1 + 45 + 225)
           = 2.67 Ã 271
           = 723.57 GB
```

Round up to 724 GB of required disk space.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page