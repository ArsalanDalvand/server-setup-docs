---
title: "Write-behind architecture"
source: "https://redis.io/docs/latest/integrate/write-behind/architecture/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/write-behind/architecture"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Write-behind architecture"
---
# Write-behind architecture

Discover the main components of Write-behind

Write-behind lets you integrate Redis Enterprise (as the source of changes to data) and downstream databases or datastores.
Write-behind captures any changes to a selected set of key patterns in a Redis keyspace and asynchronously writes them in small batches to the downstream database. This means that your app doesn't need to handle the data remodeling or manage the connection with the downstream database.

Write-behind can normalize a key in Redis to several records in one or more tables at the target.
To learn more about write-behind declarative jobs and normalization, see the
[write-behind quick start guide](/docs/latest/integrate/write-behind/quickstart/write-behind-guide/).

## Write-behind topology

Write-behind's CLI and engine are shipped as one product that can run both ingest and write-behind pipelines.
However, the two different types of pipeline have different topologies.

The Write-behind engine is installed on the Redis database that contains the application data and not on a separate staging Redis database. The Write-behind data streams and its control plane add only a small footprint of a few hundred MB to the Redis database. In the write-behind topology, Write-behind processes data in parallel on each shard and establishes a single connection from each shard to the downstream database.

### Model translation

Write-behind can track changes to the following Redis types:

[Hash](/docs/latest/develop/data-types/hashes/)
[JSON](/docs/latest/develop/data-types/json/)
[Set](/docs/latest/develop/data-types/sets/)
[Sorted Set](/docs/latest/develop/data-types/sorted-sets/)

Unlike the ingest scenario, write-behind has no default behavior for model translation. You must always
create a declarative job to specify the mapping between Redis keys and target database records.
The job configuration has `keys` and `mapping` sections that help make this an easy task.

## Write-behind components

### Write-behind CLI

Write-behind's Python-based CLI is highly intuitive to use and performs validations to help you avoid mistakes.
The CLI makes it easy to set up Write-behind and manage it over its entire lifecycle.

### Write-behind Engine

Write-behind uses Redis Gears as its runtime environment. The Gears and Write-behind engine logic are installed
on all source Redis Enterprise database shards, but only primary shards process events and handle the pipeline.

The Write-behind Engine reads Redis change events from Redis Streams (one for each tracked key-pattern),
processes them, and translates them to SQL or whatever other language the target database uses.

Write-behind writes changes to the target in small batches using transactions. Write-behind guarantees
at-least once delivery. If any network problems, disconnections, or other temporary failures occur,
Write-behind will keep attempting to write the changes to the target. If a hard reject occurs, Write-behind keeps the reject
record and the reason in a dead letter queue (DLQ).

### Write-behind configuration

The Write-behind configuration is persisted at the cluster level. The configuration is written by the CLI
[[integrate/write-behind/reference/cli/redis-di-deploy|`deploy`]]
command, which saves all changes to the configuration file. This mechanism allows for automatic configuration of new shards
whenever you need them, and it can survive shard and node failure.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page