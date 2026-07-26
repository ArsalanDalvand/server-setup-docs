---
title: "Overview"
source: "https://stalw.art/docs/storage/backends/composite"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/backends/composite"
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

      

Composite backends combine several underlying stores so that load can be distributed or redundancy increased for critical data. They improve throughput and resilience in environments with high availability or scalability requirements.

Stalwart supports three composite backends:

[[storage/backends/composite/sharded-in-memory|Sharded In-Memory Store]]: distributes key-value pairs across multiple in-memory backends (typically Redis or compatible). Keys are hashed and mapped to a specific backend via a modulus operation, which evenly distributes data and queries across the configured stores.
[[storage/backends/composite/sharded-blob|Sharded Blob Store]]: distributes large binary objects across multiple blob backends, using the same hash-and-modulus scheme. Each backend holds a portion of the overall blob namespace.
[[storage/backends/composite/sql-replica|SQL Read Replicas]]: directs write operations to a primary SQL database and distributes read operations across one or more replicas, reducing load on the primary and improving throughput for read-heavy workloads.