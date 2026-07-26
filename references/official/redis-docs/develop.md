---
title: "Develop with Redis"
source: "https://redis.io/docs/latest/develop/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop"
category: "redis"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Develop with Redis"
---
# Develop with Redis

Learn how to develop with Redis

Try these Redis commands
Redis CLI

`PING`
`HSET user:1 name antirez vocation artist`
`HGETALL user:1`
`SET e 2.71`
`INCRBYFLOAT e 0.43`
`RENAME e pi`
`GET pi`

                  # [See all commands](/commands/)

```
PING
```

Get a Redis server running in minutes with a free trial of
[[operate/rc|Redis Cloud]], or install
[[operate/oss_and_stack|Redis Open Source]] locally
on your machine. Check out the [Redis Docker Hub](https://hub.docker.com/_/redis) for the latest release. Then, explore Redis with your favorite
[[develop/clients|programming language]]
or analyze and manage your database with our
[[develop/tools|UI tools]]:

Get started
Document search
Vector search

[[develop/clients/redis-py|Python]]
[[develop/clients/redis-py/connect|See Python examples]]
[[develop/clients/redis-py/queryjson|See Python examples]]
[[develop/clients/redis-py/vecsearch|See Python examples]]

[C#/.NET](/docs/latest/develop/clients/dotnet/)
[See C# examples](/docs/latest/develop/clients/dotnet/connect/)
[See C# examples](/docs/latest/develop/clients/dotnet/nredisstack/queryjson/)
[See C# examples](/docs/latest/develop/clients/dotnet/nredisstack/vecsearch/)

[Node.js](/docs/latest/develop/clients/nodejs/)
[See JS examples](/docs/latest/develop/clients/nodejs/connect/)
[See JS examples](/docs/latest/develop/clients/nodejs/queryjson/)
[See JS examples](/docs/latest/develop/clients/nodejs/vecsearch/)

[Java (Jedis)](/docs/latest/develop/clients/jedis/)
[See Java examples](/docs/latest/develop/clients/jedis/connect/)
[See Java examples](/docs/latest/develop/clients/jedis/queryjson/)
[See Java examples](/docs/latest/develop/clients/jedis/vecsearch/)

[Java (Lettuce)](/docs/latest/develop/clients/lettuce/)
[See Java examples](/docs/latest/develop/clients/lettuce/connect/)
[See Java examples](/docs/latest/develop/clients/lettuce/queryjson/)
[See Java examples](/docs/latest/develop/clients/lettuce/vecsearch/)

[Go](/docs/latest/develop/clients/go/)
[See Go examples](/docs/latest/develop/clients/go/connect/)
[See Go examples](/docs/latest/develop/clients/go/queryjson/)
[See Go examples](/docs/latest/develop/clients/go/vecsearch/)

[PHP](/docs/latest/develop/clients/php/)
[See PHP examples](/docs/latest/develop/clients/php/connect/)
[See PHP examples](/docs/latest/develop/clients/php/queryjson/)
[See PHP examples](/docs/latest/develop/clients/php/vecsearch/)

[JavaScript (ioredis)](/docs/latest/develop/clients/ioredis/)
[See JS examples](/docs/latest/develop/clients/ioredis/connect/)
-
-

[Rust](/docs/latest/develop/clients/rust/)
-
-
-

[[develop/clients/hiredis|C]]
-
-
-

[Ruby](/docs/latest/develop/clients/ruby/)
-
-
-

[Redis Insight](/docs/latest/develop/tools/insight/)

Visual client tool for creating, managing, and analyzing Redis databases.[Download Redis Insight](https://redis.io/downloads/#insight).

[Redis for VS Code](/docs/latest/develop/tools/redis-for-vscode/)

VS Code extension for creating, managing, and analyzing Redis databases.[Install Redis for VS Code](https://marketplace.visualstudio.com/items?itemName=Redis.redis-for-vscode).

[[develop/get-started|Quick start]]

[Data types](/docs/latest/develop/data-types/)

[Redis Search](/docs/latest/develop/ai/search-and-query/)

[[develop/get-started/vector-database|Vector database]][[develop/get-started/document-database|Document store]][[develop/get-started/data-store|Data structure store]][[develop/get-started/rag|RAG with Redis]][[develop/get-started/redis-in-ai|GenAI]]
[String](/docs/latest/develop/data-types/strings/)[JSON](/docs/latest/develop/data-types/json/)[Hash](/docs/latest/develop/data-types/hashes/)[Vector set](/docs/latest/develop/data-types/vector-sets/)[Probabilistic types](/docs/latest/develop/data-types/probabilistic/)
[Get started](/docs/latest/develop/ai/search-and-query/)[Schema field types](/docs/latest/develop/ai/search-and-query/indexing/field-and-type-options/)[Indexing](/docs/latest/develop/ai/search-and-query/indexing/)[Querying](/docs/latest/develop/ai/search-and-query/query/)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)