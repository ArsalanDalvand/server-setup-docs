---
title: "Redis"
source: "https://stalw.art/docs/cluster/coordination/redis"
source_format: official-html-to-markdown
language: "en"
doc_slug: "cluster/coordination/redis"
category: "cluster"
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

      

Redis is an in-memory data structure store commonly used for caching, session storage, and ephemeral key-value data. Alongside its role as a high-speed cache, Redis provides native publish/subscribe (pub/sub) messaging, which can be used for lightweight inter-process or inter-node communication.

In a Stalwart cluster, Redis can serve as a coordination backend using pub/sub to propagate internal updates between nodes. These include mailbox changes, IMAP IDLE and push notification triggers, blocked IP alerts, and ACME certificate availability. Each node subscribes to the relevant Redis channels and publishes updates as needed, keeping the cluster synchronised in real time.

Redis is useful in small to medium deployments where Kafka or NATS may be too heavy, or where Redis is already in use for transient or ephemeral data. Using Redis for both coordination and ephemeral storage consolidates services and avoids an additional piece of infrastructure.

Redis pub/sub is designed for transient messaging and does not persist messages or guarantee delivery to offline subscribers. For most real-time coordination tasks in modestly sized mail infrastructures this is acceptable, but Redis is not suitable for high-throughput or high-reliability scenarios where message loss is unacceptable.

## Configuration

[Section titled “Configuration”](#configuration)

Redis coordination is configured through the [[ref/object/coordinator|Coordinator]] singleton (found in the WebUI under  Settings ›  Cluster › Coordinator). Two variants are available:

`Redis`: a standalone Redis server. The variant carries [[ref/object/coordinator#url|`url`]], [[ref/object/coordinator#timeout|`timeout`]], and connection-pool fields [[ref/object/coordinator#poolmaxconnections|`poolMaxConnections`]], [[ref/object/coordinator#pooltimeoutcreate|`poolTimeoutCreate`]], [[ref/object/coordinator#pooltimeoutwait|`poolTimeoutWait`]], and [[ref/object/coordinator#pooltimeoutrecycle|`poolTimeoutRecycle`]].
`RedisCluster`: a Redis Cluster deployment. The variant carries [[ref/object/coordinator#urls|`urls`]], authentication through [[ref/object/coordinator#authusername|`authUsername`]] and [[ref/object/coordinator#authsecret|`authSecret`]], retry settings [[ref/object/coordinator#maxretries|`maxRetries`]], [[ref/object/coordinator#minretrywait|`minRetryWait`]], and [[ref/object/coordinator#maxretrywait|`maxRetryWait`]], [[ref/object/coordinator#readfromreplicas|`readFromReplicas`]], [[ref/object/coordinator#protocolversion|`protocolVersion`]] (`resp2` or `resp3`), and the same connection-pool fields as the standalone variant.

The `Default` variant of Coordinator reuses the Redis connection already configured on the [[storage/in-memory|in-memory store]], avoiding the need to maintain a second connection when Redis is already used for ephemeral data. For detailed connection parameters, authentication options, TLS support, and tuning, refer to the [[storage/backends/redis|Redis in-memory store]] page.

For example, a standalone Redis coordinator:

```
{  "@type": "Redis",  "url": "redis://redis.example.com:6379",  "timeout": "10s",  "poolMaxConnections": 10}
```

A Redis Cluster coordinator with authentication and retry tuning:

```
{  "@type": "RedisCluster",  "urls": [    "redis://redis-1.example.com:6379",    "redis://redis-2.example.com:6379",    "redis://redis-3.example.com:6379"  ],  "timeout": "10s",  "authUsername": "stalwart",  "authSecret": {    "@type": "Value",    "secret": "redis-password"  },  "readFromReplicas": true,  "protocolVersion": "resp3"}
```