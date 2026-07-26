---
title: "SQL Read Replicas"
source: "https://stalw.art/docs/storage/backends/composite/sql-replica"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/backends/composite/sql-replica"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SQL Read Replicas"
---
# SQL Read Replicas

      

SQL read replicas are a well-known database architecture pattern for separating read and write traffic. A primary database handles all write operations, ensuring consistent updates; one or more read replicas, synchronised from the primary, handle read traffic. This separation reduces load on the primary and allows the read path to scale horizontally.

Stalwart supports this pattern for any SQL backend that itself supports read replicas (PostgreSQL, MySQL, MariaDB, Galera, AlloyDB, and similar). Using read replicas improves throughput in read-heavy environments while preserving write consistency.

## Configuration

[Section titled “Configuration”](#configuration)

Read replicas are configured directly on the [[storage/backends/postgresql#read-replicas|PostgreSQL]] or [[storage/backends/mysql#read-replicas|MySQL / MariaDB]] store; there is no standalone SQL-replica variant at the top level of [[ref/object/data-store|DataStore]], [[ref/object/blob-store|BlobStore]], or [[ref/object/search-store|SearchStore]]. See the backend-specific pages for the exact field list and examples.