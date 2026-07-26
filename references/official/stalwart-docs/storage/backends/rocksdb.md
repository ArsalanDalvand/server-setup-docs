---
title: "RocksDB"
source: "https://stalw.art/docs/storage/backends/rocksdb"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/backends/rocksdb"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "RocksDB"
---
# RocksDB

      

RocksDB is an embeddable, high-performance, persistent key-value store originally developed at Facebook. It is well suited to fast storage devices such as SSDs, and its efficient use of CPU and memory makes it a popular choice for embedded workloads. RocksDB is the recommended backend for single-node installations of Stalwart because of its speed and reliability.

## Configuration

[Section titled “Configuration”](#configuration)

The RocksDB backend is selected by choosing the `RocksDb` variant on the [[ref/object/data-store|DataStore]] object (found in the WebUI under  Settings ›  Storage › Data Store). The variant exposes the following fields:

[[ref/object/data-store#path|`path`]]: filesystem path where the RocksDB data directory is stored (required).
[[ref/object/data-store#blobsize|`blobSize`]]: minimum size, in bytes, for an object to be stored in the blob store rather than inline in the metadata store. Default: `16834`.
[[ref/object/data-store#buffersize|`bufferSize`]]: size of the in-memory write buffer, in bytes. A larger buffer improves write throughput at the cost of memory. Default: `134217728` (128 MB).
[[ref/object/data-store#poolworkers|`poolWorkers`]]: number of worker threads dedicated to database operations. Defaults to the number of CPU cores available on the host.

RocksDB is also available as a backend for other stores (blob, search, in-memory) through the `Default` variant of the [[ref/object/blob-store|BlobStore]], [[ref/object/search-store|SearchStore]], and [[ref/object/in-memory-store|InMemoryStore]] objects, in which case they reuse the configured data store.