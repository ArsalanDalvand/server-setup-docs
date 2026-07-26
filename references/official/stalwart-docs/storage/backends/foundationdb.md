---
title: "FoundationDB"
source: "https://stalw.art/docs/storage/backends/foundationdb"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/backends/foundationdb"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "FoundationDB"
---
# FoundationDB

      

FoundationDB is a distributed database designed to handle large volumes of data across multiple machines. It combines ACID transactions with the scalability of NoSQL systems, providing high performance, fault tolerance, and straightforward horizontal scaling. It is the recommended backend for distributed deployments of Stalwart.

## Configuration

[Section titled “Configuration”](#configuration)

The FoundationDB backend is selected by choosing the `FoundationDb` variant on the [[ref/object/data-store|DataStore]] object (found in the WebUI under  Settings ›  Storage › Data Store). The variant exposes the following fields:

[[ref/object/data-store#clusterfile|`clusterFile`]]: filesystem path to the FoundationDB cluster configuration file. If unset, the default `/etc/foundationdb/fdb.cluster` is used.
[[ref/object/data-store#transactiontimeout|`transactionTimeout`]]: duration after which a transaction times out if not completed.
[[ref/object/data-store#transactionretrylimit|`transactionRetryLimit`]]: maximum number of transaction retries before giving up.
[[ref/object/data-store#transactionretrydelay|`transactionRetryDelay`]]: maximum delay between transaction retries.
[[ref/object/data-store#machineid|`machineId`]]: optional identifier for the machine, useful for logging and cluster-aware behaviour.
[[ref/object/data-store#datacenterid|`datacenterId`]]: optional identifier for the data centre hosting the machine, used when tuning cross-datacentre replication.

FoundationDB is also available as a backend for the blob store, search store, and in-memory store; the same set of cluster-file and transaction fields is exposed on each of those objects under the `FoundationDb` variant.