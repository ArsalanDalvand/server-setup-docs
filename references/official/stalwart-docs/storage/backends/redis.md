---
title: "Redis"
source: "https://stalw.art/docs/storage/backends/redis"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/backends/redis"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Redis"
---
# Redis

      

Redis (Remote Dictionary Server) is an in-memory data-structure store used as a database, cache, and message broker. It supports a range of data structures (strings, hashes, lists, sets, sorted sets, bitmaps, hyperloglogs, geospatial indexes, streams), replication, Lua scripting, LRU eviction, transactions, and several levels of on-disk persistence. Valkey, the open-source Redis-compatible fork, is also supported through the same variant.

## Standalone configuration

[Section titled “Standalone configuration”](#standalone-configuration)

The standalone Redis backend is selected by choosing the `Redis` variant on the [[ref/object/in-memory-store|InMemoryStore]] object (found in the WebUI under  Settings ›  Storage › In-Memory Store). The variant exposes the following fields:

[[ref/object/in-memory-store#url|`url`]]: connection URL for the Redis server. Default: `"redis://127.0.0.1"`. The URL may embed a username, password, port, and database index: `redis[s]://[<username>][:<password>@]<hostname>[:port][/<db>]#insecure`. The `#insecure` fragment disables TLS verification when using the `rediss` scheme, which is useful with self-signed certificates.
[[ref/object/in-memory-store#timeout|`timeout`]]: maximum time to wait for a response from the Redis server. Default: `"10s"`.
[[ref/object/in-memory-store#poolmaxconnections|`poolMaxConnections`]]: maximum number of pooled connections. Default: `10`.
[[ref/object/in-memory-store#pooltimeoutcreate|`poolTimeoutCreate`]], [[ref/object/in-memory-store#pooltimeoutwait|`poolTimeoutWait`]], [[ref/object/in-memory-store#pooltimeoutrecycle|`poolTimeoutRecycle`]]: connection-pool lifecycle timeouts. Default: `"30s"` each.

## Cluster configuration

[Section titled “Cluster configuration”](#cluster-configuration)

The Redis Cluster backend is selected by choosing the `RedisCluster` variant on the [[ref/object/in-memory-store|InMemoryStore]] object. The variant exposes the following fields:

[[ref/object/in-memory-store#urls|`urls`]]: list of connection URLs for the cluster nodes.
[[ref/object/in-memory-store#authusername|`authUsername`]]: username used to authenticate. Default: `"stalwart"`.
[[ref/object/in-memory-store#authsecret|`authSecret`]]: password or other secret reference used to authenticate (required).
[[ref/object/in-memory-store#timeout|`timeout`]]: maximum time to wait for a response from the Redis server. Default: `"10s"`.
[[ref/object/in-memory-store#maxretries|`maxRetries`]], [[ref/object/in-memory-store#minretrywait|`minRetryWait`]], [[ref/object/in-memory-store#maxretrywait|`maxRetryWait`]]: command retry policy controlling the total number of retries and the backoff between attempts.
[[ref/object/in-memory-store#readfromreplicas|`readFromReplicas`]]: when `true`, allows reads to be served by replica nodes; when `false`, reads are restricted to the primary. Default: `true`.
[[ref/object/in-memory-store#protocolversion|`protocolVersion`]]: Redis protocol version. Supported values are `resp2` and `resp3`. Default: `resp2`.
The pool lifecycle fields ([[ref/object/in-memory-store#poolmaxconnections|`poolMaxConnections`]], [[ref/object/in-memory-store#pooltimeoutcreate|`poolTimeoutCreate`]], [[ref/object/in-memory-store#pooltimeoutwait|`poolTimeoutWait`]], [[ref/object/in-memory-store#pooltimeoutrecycle|`poolTimeoutRecycle`]]) mirror those of the standalone variant.

To distribute load across several independent Redis deployments, combine multiple Redis or Redis Cluster backends through the [[storage/backends/composite/sharded-in-memory|Sharded in-memory store]].