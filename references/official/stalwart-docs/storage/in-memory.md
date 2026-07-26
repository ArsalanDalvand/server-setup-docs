---
title: "In-memory store"
source: "https://stalw.art/docs/storage/in-memory"
source_format: official-html-to-markdown
language: "en"
doc_slug: "storage/in-memory"
category: "storage"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "In-memory store"
---
# In-memory store

      

In-memory data stores (such as Redis) are high-performance databases that keep data entirely in RAM, making read and write operations extremely fast. They are well suited to caching, message brokering, and short-lived state that does not need long-term persistence.

Within Stalwart, the in-memory store backs a range of short-lived data:

[[mta/outbound/rate-limit|Rate limiting]] and [[server/auto-ban|fail2ban]] counters, such as the number of messages sent by a given sender or the number of failed authentication attempts from an IP.
Distributed locks that coordinate concurrent tasks, including account purge, email queue processing, and housekeeping.
[[auth/oauth|OAuth authorisation codes]] used during the authorisation flow.
[[server/tls/acme|ACME tokens]] for SSL/TLS certificate management.
Short-lived tracking data such as [[spamfilter/greylist|greylisting]] tokens and [[sieve|Sieve]] auto-responder IDs.

## Backends

[Section titled “Backends”](#backends)

Backend selection is made on the [[ref/object/in-memory-store|InMemoryStore]] singleton (found in the WebUI under  Settings ›  Storage › In-Memory Store). The object is a multi-variant type. The supported variants are:

[[storage/backends/redis|Redis, Valkey, Memcached, or compatible]]: the `Redis` and `RedisCluster` variants, recommended for high-performance, distributed, or heavily loaded environments.
The `Default` variant, which uses the configured data store (SQL, FoundationDB, RocksDB) as the in-memory store. This is a convenient choice when minimising external dependencies, though Redis is better suited to heavy write loads and distributed setups.
The `Sharded` variant, which distributes key-value pairs across multiple Redis or Redis Cluster backends by hashing keys modulo the number of configured stores. See the [[storage/backends/composite/sharded-in-memory|Sharded in-memory store]] page for further detail.

## Key prefixes

[Section titled “Key prefixes”](#key-prefixes)

In-memory stores operate as key-value stores. Stalwart assigns a unique integer prefix to each category of data so that keys never collide across subsystems. The mapping is fixed and shown below for reference:

Short nameDescriptionInteger prefix`KV_ACME`ACME tokens for SSL/TLS certificate management0`KV_OAUTH`OAuth authorisation codes1`KV_RATE_LIMIT_RCPT`Rate limiting data for recipient addresses2`KV_RATE_LIMIT_SCAN`Rate limiting data for email scanning3`KV_RATE_LIMIT_LOITER`Rate limiting data for idle connections4`KV_RATE_LIMIT_AUTH`Rate limiting data for authentication attempts5`KV_RATE_LIMIT_SMTP`Rate limiting data for SMTP throttles6`KV_RATE_LIMIT_CONTACT`Rate limiting data for contact forms7`KV_RATE_LIMIT_HTTP_AUTHENTICATED`Rate limiting data for authenticated HTTP requests8`KV_RATE_LIMIT_HTTP_ANONYMOUS`Rate limiting data for anonymous HTTP requests9`KV_RATE_LIMIT_IMAP`Rate limiting data for IMAP connections10`KV_QUOTA_BLOB`Blob-quota accounting11`KV_GREYLIST`Spam filter greylist tokens16`KV_LOCK_QUEUE_MESSAGE`Lock for SMTP message queues21`KV_LOCK_TASK`Lock for background tasks23`KV_LOCK_DAV`WebDAV locks25`KV_SIEVE_ID`Sieve auto-responder tracking ids26

This structured approach ensures data integrity and prevents key collisions across subsystems within the in-memory store.

## Data persistence

[Section titled “Data persistence”](#data-persistence)

Most keys held in the in-memory store are transient: rate-limit counters, fail2ban records, authorisation codes, and short-lived tokens do not need to survive a restart. Configuring persistence on the backend is therefore not usually required.

## Configuration

[Section titled “Configuration”](#configuration)

To change the in-memory store backend, update the InMemoryStore singleton and select the appropriate variant. Variant-specific fields such as [[ref/object/in-memory-store#url|`url`]] and [[ref/object/in-memory-store#timeout|`timeout`]] apply to the Redis variant; [[ref/object/in-memory-store#urls|`urls`]], [[ref/object/in-memory-store#authusername|`authUsername`]], [[ref/object/in-memory-store#authsecret|`authSecret`]], [[ref/object/in-memory-store#readfromreplicas|`readFromReplicas`]], and [[ref/object/in-memory-store#protocolversion|`protocolVersion`]] apply to the RedisCluster variant. See the [[ref/object/in-memory-store|InMemoryStore reference]] for the full field list per variant.

## Lookup keys for expressions and Sieve

[Section titled “Lookup keys for expressions and Sieve”](#lookup-keys-for-expressions-and-sieve)

Lookup data consumed by expressions and Sieve scripts is kept in the same InMemoryStore singleton; there is no separate named store per lookup. Individual entries are modelled as [[ref/object/memory-lookup-key|MemoryLookupKey]] (found in the WebUI under  Settings ›  Lookups › In-Memory Keys,  Settings ›  Spam Filter › Lists › Blocked Domains,  Settings ›  Spam Filter › Lists › Spam Traps,  Settings ›  Spam Filter › Lists › Trusted Domains,  Settings ›  Spam Filter › Lists › URL Redirectors) for membership-style lookups and as [[ref/object/memory-lookup-key-value|MemoryLookupKeyValue]] (found in the WebUI under  Settings ›  Lookups › In-Memory Key-Values) for key-value lookups. Each record carries its own [[ref/object/memory-lookup-key#namespace|`namespace`]], so distinct lookup sets coexist within the single backend.

## Maintenance

[Section titled “Maintenance”](#maintenance)

When a persistent data store (SQL, FoundationDB, RocksDB) is used as the in-memory store through the `Default` variant, expired keys must be purged periodically. This is handled by the general data-store clean-up task, scheduled through [[ref/object/data-retention#datacleanupschedule|`dataCleanupSchedule`]] on the [[ref/object/data-retention|DataRetention]] object (found in the WebUI under  Settings ›  Storage › Data Retention › Archiving,  Settings ›  Storage › Data Retention › Data Cleanup,  Settings ›  Storage › Data Retention › Auto-Expunge,  Settings ›  Storage › Data Retention › Telemetry). When Redis or Redis Cluster is used, expiry is handled natively by the backend and no scheduled clean-up is required.