---
title: "Overview"
source: "https://stalw.art/docs/storage/backends"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/backends"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Overview"
---
# Overview

      

Stalwart supports several database and object-storage backends. The choice of backend depends primarily on the scale and topology of the deployment.

The following table summarises which backend is available for each store type:

Data storeBlob storeSearch storeIn-memory storeRocksDB✅✅✅✅FoundationDB✅✅✅✅PostgreSQL✅✅✅✅MySQL / MariaDB✅✅✅✅SQLite✅✅✅✅S3/MinIO✅Azure Blob Storage✅Filesystem✅ElasticSearch✅Meilisearch✅Redis✅

Backend support is compiled into the Stalwart binary. Switching to backends such as FoundationDB therefore requires running a different binary built with the corresponding feature enabled.

Each store (data, blob, search, in-memory) is configured through its own singleton object: [[ref/object/data-store|DataStore]], [[ref/object/blob-store|BlobStore]], [[ref/object/search-store|SearchStore]], and [[ref/object/in-memory-store|InMemoryStore]]. These objects are multi-variant: changing backend means selecting a different variant on the corresponding object, without restructuring the rest of the configuration.