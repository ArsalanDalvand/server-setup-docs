---
title: "Sharded Blob Store"
source: "https://stalw.art/docs/storage/backends/composite/sharded-blob"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/backends/composite/sharded-blob"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Sharded Blob Store"
---
# Sharded Blob Store

      

Sharding distributes data across multiple backends to improve scalability and balance load. In the sharded blob store, each blob’s key is hashed and a modulus operation selects one of the configured backends to hold or retrieve it. Each backend therefore owns a deterministic portion of the blob namespace, and reads and writes for a given blob always resolve to the same backend.

## Configuration

[Section titled “Configuration”](#configuration)

The sharded blob backend is selected by choosing the `Sharded` variant on the [[ref/object/blob-store|BlobStore]] object (found in the WebUI under  Settings ›  Storage › Blob Store). The variant exposes a single field:

[[ref/object/blob-store#stores|`stores`]]: list of two or more backend blob stores that make up the shard. Each entry selects one of the base blob-store variants (S3, Azure, filesystem, FoundationDB, PostgreSQL, or MySQL).

IMPORTANT: the order and number of backends in [[ref/object/blob-store#stores|`stores`]] must remain stable once set. Reordering or adding or removing entries changes the hash-to-store mapping and leaves previously stored blobs unreachable. Plan capacity carefully before configuring a shard.