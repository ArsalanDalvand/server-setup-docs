---
title: "Sharded In-Memory Store"
source: "https://stalw.art/docs/storage/backends/composite/sharded-in-memory"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/backends/composite/sharded-in-memory"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Sharded In-Memory Store"
---
# Sharded In-Memory Store

      

Sharding distributes data across multiple backends to improve throughput and balance load. In the sharded in-memory store, each key is hashed and a modulus operation selects one of the configured backends to hold or serve the associated value. Each backend owns a deterministic subset of keys, which avoids hotspots and allows capacity to scale horizontally.

## Configuration

[Section titled “Configuration”](#configuration)

The sharded in-memory backend is selected by choosing the `Sharded` variant on the [[ref/object/in-memory-store|InMemoryStore]] object (found in the WebUI under  Settings ›  Storage › In-Memory Store). The variant exposes a single field:

[[ref/object/in-memory-store#stores|`stores`]]: list of two or more backend in-memory stores that make up the shard. Each entry selects one of the base in-memory variants (Redis or Redis Cluster).

IMPORTANT: the order and number of backends in [[ref/object/in-memory-store#stores|`stores`]] must remain stable once set. Reordering or adding or removing entries changes the hash-to-store mapping and leaves previously written data unreachable. Plan capacity carefully before configuring a shard.