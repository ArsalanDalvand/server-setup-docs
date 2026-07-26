---
title: "What's new?"
source: "https://redis.io/docs/latest/develop/whats-new/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/whats-new"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "What's new?"
---
# What's new?

High-level description of important updates to the Develop section

## Q2 2026 (April - June) Updates

### Redis Version Updates

[[develop/whats-new/8-8|Redis 8.8]] - New Array data structure, `INCREX` rate limiter, in-group sorting reducer for `FT.AGGREGATE`, Rust iterators and devirtualized vector hot paths in Search, HyperLogLog and `MGET`/`MSET`/`HGETALL` performance improvements.

## Q1 2026 (January - March) Updates

### Tools

Redis Insight [v3.2.0 release notes](/docs/latest/develop/tools/insight/release-notes/v.3.2.0/)
Redis Insight [v3.0.3 release notes](/docs/latest/develop/tools/insight/release-notes/v.3.0.3/)
Redis Insight [v2.68.0 release notes](/docs/latest/develop/tools/insight/release-notes/v.2.68.0/)
Redis Insight [v2.66.0 release notes](/docs/latest/develop/tools/insight/release-notes/v.2.66.0/)

### Redis AI & Vectors

Added [[integrate/n8n-vector-store|n8n vector store integration]]
Updated [[develop/whats-new|RedisVL 0.7.0 documentation]]
Updated [[develop/whats-new|RedisVL 0.9.1 documentation]]
Updated [[develop/whats-new|RedisVL 0.12.1 documentation]] (28 pages)

### Data Types

Enhanced [example progression](/docs/latest/develop/data-types/) with metadata for:

[Strings](/docs/latest/develop/data-types/strings/)
[Hashes](/docs/latest/develop/data-types/hashes/)
[Lists](/docs/latest/develop/data-types/lists/)
[Sets](/docs/latest/develop/data-types/sets/)
[Sorted sets](/docs/latest/develop/data-types/sorted-sets/)
[Bitmaps](/docs/latest/develop/data-types/strings/bitmaps/)
[Geospatial](/docs/latest/develop/data-types/geospatial/)
[JSON](/docs/latest/develop/data-types/json/)
[Time series](/docs/latest/develop/data-types/timeseries/)
[Vector sets](/docs/latest/develop/data-types/vector-sets/)

### Client Libraries

#### Ruby (NEW)

Added [Ruby client documentation](/docs/latest/develop/clients/ruby/)
Landing page code examples for redis-rb

#### ioredis (NEW)

Added [ioredis connect page](/docs/latest/develop/clients/ioredis/connect/)

#### node-redis (JavaScript)

Added Smart client handoffs section in the Connect page

#### Rust

Added [Rust AMR/EntraID connection page](/docs/latest/develop/clients/rust/amr/)
Added Rust error handling documentation
Added Rust transactions and pipelines documentation

#### Python (redis-py)

Updated failover documentation with initial health checks
Added failback configuration details
Added observability page
Added Smart client handoffs section in the Connect page

#### Java (Jedis)

Added failback configuration section
Updated failover behavior documentation

#### Java (Lettuce)

Added failover documentation
Added `LagAwareStrategy` dependencies
Added Smart client handoffs section in the Connect page

#### .NET (NRedisStack)

Added [[integrate/fusioncache|FusionCache integration page]]

#### Go

Added observability page
Added Smart client handoffs section in the Connect page

### Documentation Updates

Added BinderHub support for "Run in browser" functionality with Python examples
Added Jupyter notebook support with pre-configured environments
Enhanced example progression metadata across all data types
Added testable code example shortcodes with language-specific markers
Fixed fuzzy search documentation with specific attribute examples
Updated client library description differences across all major clients
Added observability overview with OpenTelemetry metrics
Added Smart client handoffs overview
Docs for n8n Redis vector store integration
Added railroad diagrams and API methods to all command pages

### Redis Version Updates

[[develop/whats-new/8-6|Redis 8.6]] - Stream idempotency, least recently modified eviction policies, hot key detection, TLS certificate-based authentication, enhanced time series with NaN support, memory optimizations
[[develop/whats-new/8-4|Redis 8.4]] - Updated documentation (removed RC1 designation)

## Q4 2025 (October - December) Updates

### Tools

Enhanced Redis Insight documentation with autocomplete features

### Redis AI & Vectors

Launched [AI Agent Builder](/docs/latest/develop/ai/agent-builder/) - interactive code generator for production-ready AI agents
Documented [RedisVL 0.9.0](/docs/latest/develop/ai/redisvl/) (28 pages):

Vector search and indexing
Semantic caching
Message history management
Rerankers and routers
Vectorizers

Added [AI video tutorials](/docs/latest/develop/ai/ai-videos/) with YouTube content
Added [AI notebook collection](/docs/latest/develop/ai/notebook-collection/) with notebook links
Added [LangCache documentation](/docs/latest/develop/ai/context-engine/langcache/) with API examples

### Client Libraries

#### Rust (NEW)

Added [Rust client documentation](/docs/latest/develop/clients/rust/)
Enabled Rust-Sync and Rust-Async client variants
Landing page code examples for Rust

#### Java (Jedis)

Added [vector set embedding examples](/docs/latest/develop/clients/jedis/vecsets/)

#### .NET (NRedisStack)

Added [vector set embedding examples](/docs/latest/develop/clients/dotnet/vecsets/)
Enabled C#-Sync and C#-Async client variants with async examples

### Documentation Updates

Integrated BinderHub for "Run in browser" functionality with Python examples
Launched Testable Code Examples (TCE) framework for hands-on learning
Added new integration guides (cloud platforms, monitoring tools, AI/ML frameworks)

## Q3 2025 (July - September) Updates

### Tools

Added [[develop/tools/cli|redis-cli installation instructions]]

### Redis AI & Vectors

Updated [vector types documentation](/docs/latest/develop/ai/search-and-query/vectors/) for INT8 and UINT8
Added [GEO search precision information](/docs/latest/develop/ai/search-and-query/indexing/geoindex/)
Enhanced [LangCache SDK](/docs/latest/develop/ai/context-engine/langcache/) with savings calculator and improved API documentation

### Data Types

[Bitmaps](/docs/latest/develop/data-types/strings/bitmaps/):

Added BITOP documentation with Python testable code examples
Added bit operation diagrams

### Client Libraries

#### Java (Jedis)

Added [vector set embedding examples](/docs/latest/develop/clients/jedis/vecsets/)

#### .NET (NRedisStack)

Added [vector set embedding examples](/docs/latest/develop/clients/dotnet/vecsets/)
Enabled C#-Sync and C#-Async client variants with async examples on the landing page

#### Rust (NEW)

Added initial [Rust client documentation](/docs/latest/develop/clients/rust/)
Enabled Rust-Sync and Rust-Async client variants
Landing page code examples for Rust

### Documentation Updates

Added [[develop/clients|close/quit details]] to client landing pages
Added [[develop/clients/patterns/distributed-locks|redlock-universal]] to Redlock implementations list
Fixed [[develop/pubsub/keyspace-notifications|keyspace notifications]] typo
Updated Unicode quotes to ASCII throughout documentation
Fixed broken links and improved cross-references

## Q2 2025 (April - June) Updates

### Tools

Redis Insight [v2.70.1 release notes](/docs/latest/develop/tools/insight/release-notes/v.2.70.1/)
Redis Insight [v1.4.0 release notes](/docs/latest/develop/tools/insight/release-notes/v1.4.0/)
Updated [Redis Insight pages](/docs/latest/develop/tools/insight/) with consistent image-card layout
Added Redis Insight SVG icons and download links across [[develop/tools|tools documentation]]

### Redis AI & Vectors

Reorganized [search and query documentation](/docs/latest/develop/ai/search-and-query/) under AI section
Added [AI video tutorials](/docs/latest/develop/ai/ai-videos/) with YouTube content
Added [AI notebook collection](/docs/latest/develop/ai/notebook-collection/) with 8 new notebook links
Expanded vector examples across multiple clients:

[[develop/clients/redis-py/vecsets|Python vector sets]]
[Go vector sets](/docs/latest/develop/clients/go/vecsets/)
[JavaScript vector sets](/docs/latest/develop/clients/nodejs/vecsets/)
[Lettuce vector queries](/docs/latest/develop/clients/lettuce/vecsearch/)
[Lettuce vector sets](/docs/latest/develop/clients/lettuce/vecsets/)

Updated [redisvl documentation](/docs/latest/develop/ai/redisvl/) for versions 0.6.0-0.8.2
Added [LangCache SDK](/docs/latest/develop/ai/context-engine/langcache/) documentation with [API reference](/docs/latest/develop/ai/context-engine/langcache/api-examples/)

### Redis 8.0 & 8.2 Features

[Redis Open Source 8.2 documentation](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisce/redisos-8.2-release-notes/)
Updated [Redis 8.0 release notes](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisce/redisos-8.0-release-notes/)
[Redis Search expiration capabilities](/docs/latest/develop/ai/search-and-query/advanced-concepts/expiration/) in Redis 8
Enhanced [TAG documentation](/docs/latest/develop/ai/search-and-query/advanced-concepts/tags/) per GitHub issues
[Vector quantization and compression](/docs/latest/develop/ai/search-and-query/vectors/svs-compression/) moved to dedicated page

### Data Types

TimeSeries:

[Enhanced time series examples](/docs/latest/develop/data-types/timeseries/) with query and aggregation info
Added testable code examples (TCE) support

Probabilistic:

Added testable examples for [[develop/clients/redis-py/prob|Python]], [C#](/docs/latest/develop/clients/dotnet/nredisstack/prob/), [Java](/docs/latest/develop/clients/jedis/prob/), and [Go](/docs/latest/develop/clients/go/prob/)
Updated [Cuckoo filter documentation](/docs/latest/develop/data-types/probabilistic/cuckoo-filter/)

### Client Libraries

#### Python (redis-py)

Added [[develop/clients/redis-py/connect#retrying-connections|reconnection examples]]
Enhanced [[develop/clients/redis-py/produsage#timeouts|timeout and retry details]]
Fixed [[develop/clients/redis-py/amr|pip install command]] for redis-py entraid extension
Added [[develop/clients/redis-py#connect-and-test|Binder environment links]] for Python examples

#### Java (Jedis)

Updated to [latest Jedis version](/docs/latest/develop/clients/jedis/#install)
Added [reconnection examples](/docs/latest/develop/clients/jedis/connect/#retrying-a-command-after-a-connection-failure)
Enhanced [probabilistic data type examples](/docs/latest/develop/clients/jedis/prob/)

#### Node.js

Added [command reliability information](/docs/latest/develop/clients/nodejs/produsage/#command-execution-reliability)
Fixed [reconnection details](/docs/latest/develop/clients/nodejs/connect/#reconnect-after-disconnection)

#### .NET (StackExchange.Redis/NRedisStack)

Added [retries and timeouts](/docs/latest/develop/clients/dotnet/produsage/) to production usage advice
Enhanced [dialect 2 notes](/docs/latest/develop/clients/dotnet/nredisstack/queryjson/)

#### Go (go-redis)

Added [retries and timeouts](/docs/latest/develop/clients/go/produsage/) to production usage
Enhanced [dialect 2 notes](/docs/latest/develop/clients/go/queryjson/)
Added [Connect with AMR](/docs/latest/develop/clients/go/amr/) page.

#### Lettuce

Updated to [latest Lettuce version](/docs/latest/develop/clients/lettuce/#install)
Added [command reliability information](/docs/latest/develop/clients/lettuce/produsage/#connection-and-execution-reliability)
Added [JSON query examples](/docs/latest/develop/clients/lettuce/queryjson/)

#### PHP (Predis)

Enhanced [dialect 2 notes](/docs/latest/develop/clients/php/queryjson/)

### Documentation Structure & Navigation

Reorganized [[develop|develop section navigation]] with improved sidebar structure
Moved [[develop/programmability|programmability section]] into develop area
Relocated [[develop/clients/patterns|patterns folder]] to clients section
Added [[develop/using-commands|Using commands section]] to develop area
Enhanced [[develop/pubsub/keyspace-notifications|keyspace notifications]] and [[develop/pubsub|pub/sub]] documentation
Updated [[develop/using-commands/transactions|transactions]] and [[develop/using-commands/pipelining|pipeline]] pages
Added comprehensive aliases for backward compatibility

### Protocol & Technical Updates

Fixed [[develop/reference/protocol-spec|RESP protocol specification]] attribute byte documentation
Enhanced [FT.AGGREGATE expression precedence](/docs/latest/develop/ai/search-and-query/advanced-concepts/aggregations-syntax/) documentation
Updated [[develop/clients/patterns/distributed-locks|distributed locks]] documentation
Fixed [FP32 vectorsets endianness](/docs/latest/develop/data-types/vector-sets/#endianness-considerations-for-fp32-format) documentation

## Q1 2025 (January - March) Updates

### Tools

Redis Insight [v2.66 release notes](/docs/latest/develop/tools/insight/release-notes/v.2.66.0/)
Updated CLI output samples for [[develop/tools/cli|`bigkeys`, `memkeys`, `keystats`]]

### Redis AI & Vectors

Expanded vector examples:

[[develop/clients/redis-py/vecsearch|Python]]
[Node.js](/docs/latest/develop/clients/nodejs/vecsearch/)
[Java (Jedis)](/docs/latest/develop/clients/jedis/vecsearch/)
[Go](/docs/latest/develop/clients/go/vecsearch/)
[.NET](/docs/latest/develop/clients/dotnet/nredisstack/vecsearch/)

Updated AI integrations:

[AI overview](/docs/latest/develop/ai/)
[[develop/get-started/rag|RAG intro]]
[[develop/get-started/redis-in-ai|Redis in AI]]

### Data Types

TimeSeries:

[`COMPACTION_POLICY`](/docs/latest/develop/data-types/timeseries/configuration/)
[[develop/clients/client-side-caching|Client-side caching update]]

JSON:

[Active memory defragmentation](/docs/latest/operate/oss_and_stack/stack-with-enterprise/json/commands/)

Probabilistic:

[Bloom filter](/docs/latest/develop/data-types/probabilistic/bloom-filter/)
[Count-min sketch](/docs/latest/develop/data-types/probabilistic/count-min-sketch/)
[Top-K](/docs/latest/develop/data-types/probabilistic/top-k/)
[Cuckoo filter](/docs/latest/develop/data-types/probabilistic/cuckoo-filter/)

### Commands & API Docs

Pages updated for format and accuracy:

[ACL SETUSER](/docs/latest/commands/acl-setuser/)
[JSON.GET](/docs/latest/commands/json.get/)
[TS.ADD](/docs/latest/commands/ts.add/)
[SCAN](/docs/latest/commands/scan/)
[SORT](/docs/latest/commands/sort/)

RESP3 reply types documented in [[develop/clients/hiredis/issue-commands|Hiredis command page]]
[[develop/clients/client-side-caching|CSC behavior clarified]]

### Search & Query

Best practices:

[Dev-to-prod guide](/docs/latest/develop/ai/search-and-query/best-practices/dev-to-prod-best-practices/)
[Scalable queries](/docs/latest/develop/ai/search-and-query/best-practices/scalable-query-best-practices/)
[Index lifecycle](/docs/latest/develop/ai/search-and-query/best-practices/index-mgmt-best-practices/)

New/updated topics:

[Autocomplete](/docs/latest/develop/ai/search-and-query/advanced-concepts/autocomplete/)
[Escaping & tokenization](/docs/latest/develop/ai/search-and-query/advanced-concepts/escaping/)
[Geo indexing](/docs/latest/develop/ai/search-and-query/indexing/geoindex/)
[Sorting, scoring, stemming](/docs/latest/develop/ai/search-and-query/advanced-concepts/sorting/)

### Client Libraries

#### Go

[Trans/pipe examples](/docs/latest/develop/clients/go/transpipe/)
[JSON queries](/docs/latest/develop/clients/go/queryjson/)

#### .NET

[Vector search](/docs/latest/develop/clients/dotnet/nredisstack/vecsearch/)
[Trans/pipe usage](/docs/latest/develop/clients/dotnet/transpipe/)
[JSON queries](/docs/latest/develop/clients/dotnet/nredisstack/queryjson/)

#### Java (Jedis)

[Vector search](/docs/latest/develop/clients/jedis/vecsearch/)
[Trans/pipe usage](/docs/latest/develop/clients/jedis/transpipe/)

#### Node.js

[Vector queries](/docs/latest/develop/clients/nodejs/vecsearch/)
[Trans/pipe examples](/docs/latest/develop/clients/nodejs/transpipe/)
[JSON queries](/docs/latest/develop/clients/nodejs/queryjson/)

#### Redis-py

[[develop/clients/redis-py/scaniter|ScanIter usage]]
[[develop/clients/redis-py/vecsearch|Vector search]]
[[develop/clients/redis-py/transpipe|Trans/pipe usage]]
[[develop/clients/redis-py/queryjson|JSON queries]]

#### Lettuce

[Cluster connection](/docs/latest/develop/clients/lettuce/connect/)
[Production usage](/docs/latest/develop/clients/lettuce/produsage/)

#### Hiredis

Full client guide:

[[develop/clients/hiredis|Overview]]
[[develop/clients/hiredis/connect|Connect]]
[[develop/clients/hiredis/issue-commands|Issue commands]]
[Handle replies](/docs/latest/develop/clients/hiredis/handle-replies/)
[[develop/clients/hiredis/transpipe|Transactions and pipelines]]

## Q4 2024 (October - December) Updates

Updated the RESP3 specification document to include the [[develop/reference/protocol-spec#attributes|attribute type]].
Updates to the [[develop/reference/eviction|key eviction]] page.
Updates to the Redis Insight page related to its new Redis Search auto-completion [feature](/docs/latest/develop/tools/insight/#workbench).
Restructured and added testable connection examples to the [[develop/clients|client pages]].
Added [Redis Open Source](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisce/) and [Redis Stack](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisstack/) release notes.
Added new [Redis for AI](/docs/latest/develop/ai/) page.
Added new [Predis (PHP client library)](/docs/latest/develop/clients/php/) page.

## Q3 2024 (July - September) Updates

Updated the [[develop/get-started/rag|RAG with Redis quick start guide]].
Updates for [Redis Open Source version 7.4](/docs/latest/operate/oss_and_stack/stack-with-enterprise/release-notes/redisce/).
Added new [Redis Insight debugging](/docs/latest/develop/tools/insight/debugging/) page.
Completed a major re-write/restructuring of the [vector indexing page](/docs/latest/develop/ai/search-and-query/vectors/).
Added new [[develop/clients/client-side-caching|client-side caching page]].
Added new documentation for the [RDI in Redis Insight feature](/docs/latest/develop/tools/insight/rdi-connector/).
Added new documentation for the [Redis for VS Code feature](/docs/latest/develop/tools/redis-for-vscode/).
Added multi-language code examples to Redis Search [query](/docs/latest/develop/ai/search-and-query/query/) pages.
Added client-side caching information to the [[develop/clients/client-side-caching#which-client-libraries-support-client-side-caching|supported clients]] pages.
Numerous changes to the [[develop/clients|Redis client content]].

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page