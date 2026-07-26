---
title: "Data store"
source: "https://stalw.art/docs/storage/data"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/data"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Data store"
---
# Data store

      

The data store holds email metadata, mailbox state, account settings, folders, and most configuration objects. It does not hold raw message bodies, Sieve scripts, or other large binary files; those are managed separately by the [[storage/blob|blob store]] so that each layer can be tuned independently.

Backend selection is made on the [[ref/object/data-store|DataStore]] singleton (found in the WebUI under  Settings ›  Storage › Data Store). The object is a multi-variant type: each instance selects one of several database backends, and the chosen variant determines which fields apply. The supported variants are:

[[storage/backends/rocksdb|RocksDB]]: a high-performance embedded key-value database, recommended for single-node installations.
[[storage/backends/foundationdb|FoundationDB]]: a distributed ACID database for clustered deployments, recommended for multi-node installations.
[[storage/backends/postgresql|PostgreSQL]]: an object-relational database.
[[storage/backends/mysql|MySQL / MariaDB]]: a relational database management system.
[[storage/backends/sqlite|SQLite]]: a self-contained, serverless, file-based database.

## Configuration

[Section titled “Configuration”](#configuration)

To change the data store backend, update the DataStore singleton and select the variant for the desired backend. Each variant carries its own fields (for example, the RocksDB variant requires [[ref/object/data-store#path|`path`]], while the PostgreSQL variant requires [[ref/object/data-store#host|`host`]], [[ref/object/data-store#database|`database`]], [[ref/object/data-store#authusername|`authUsername`]], and [[ref/object/data-store#authsecret|`authSecret`]]). See the [[ref/object/data-store|DataStore reference]] for the full field list per variant.

## Maintenance

[Section titled “Maintenance”](#maintenance)

Scheduled clean-up of the data store is managed centrally through the [[ref/object/data-retention|DataRetention]] object (found in the WebUI under  Settings ›  Storage › Data Retention › Archiving,  Settings ›  Storage › Data Retention › Data Cleanup,  Settings ›  Storage › Data Retention › Auto-Expunge,  Settings ›  Storage › Data Retention › Telemetry). The [[ref/object/data-retention#datacleanupschedule|`dataCleanupSchedule`]] field sets how often the data store clean-up task runs, using a cron expression. The default runs daily at 02:00. Related schedules include [[ref/object/data-retention#expungeschedule|`expungeSchedule`]] for auto-expunge and [[ref/object/data-retention#blobcleanupschedule|`blobCleanupSchedule`]] for blob store clean-up.