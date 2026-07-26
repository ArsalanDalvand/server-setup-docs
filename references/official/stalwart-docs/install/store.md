---
title: "Choosing a database"
source: "https://stalw.art/docs/install/store"
source_format: official-html-to-markdown
language: "en"
doc_slug: "install/store"
category: "install"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Choosing a database"
---
# Choosing a database

      

Stalwart employs a modular storage architecture that separates different types of data into four distinct stores. This design provides flexibility and allows each storage component to be optimized independently, depending on the scale and needs of the deployment.

The [[storage/data|Data Store]] is responsible for storing structured metadata such as email headers, folder hierarchies, calendar, contacts, user settings, and other non-binary information. Essentially, it holds everything except large binary objects.
The [[storage/blob|Blob Store]] is where large binary files (such as the actual email content, sieve scripts, and attachments) are stored.
To support efficient search operations, Stalwart also utilizes a [[storage/fts|Search Store]], which maintains indexes to speed up text-based queries across message content.
Finally, the [[storage/in-memory|In-memory Store]] handles ephemeral key-value data used by security, clustering, and anti-spam components, including rate limiting, distributed locks and message interaction tracking.

In simple or single-node setups, it is entirely feasible to consolidate these roles into a single storage backend. For example, databases such as RocksDB or PostgreSQL can be configured to serve as the data store, blob store, search store, and even in-memory store. It is common to see lightweight deployments where RocksDB is used exclusively for all four functions, simplifying management and reducing infrastructure complexity.

However, for larger deployments (particularly those running in [[cluster|distributed environments]]), it is advisable to assign each storage role to a backend that is specifically designed for that purpose. A more scalable configuration might use FoundationDB as the data store, an S3-compatible service for blob storage, Redis for volatile in-memory data, and a dedicated search engine such as Elasticsearch or Meilisearch for full-text indexing.

Selecting the right combination of databases will depend on the performance requirements, fault tolerance expectations, and operational constraints of the deployment. Stalwart’s flexible architecture ensures that it can scale from compact single-server setups to complex multi-node clusters with specialized infrastructure for each storage layer.

The following table summarizes the supported backends available for each store type:

Data storeBlob storeSearch storeIn-memory storeRocksDB✅✅✅✅FoundationDB✅✅✅✅PostgreSQL✅✅✅✅MySQL / MariaDB✅✅✅✅SQLite✅✅✅✅S3/MinIO✅Azure Blob Storage✅Filesystem✅ElasticSearch✅Meilisearch✅Redis / Valkey✅