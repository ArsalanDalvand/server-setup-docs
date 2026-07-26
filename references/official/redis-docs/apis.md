---
title: "APIs"
source: "https://redis.io/docs/latest/apis/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "apis"
category: "redis"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "APIs"
---
# APIs

An overview of Redis APIs for developers and operators

Redis provides a number of APIs for developers and operators. The following sections provide you easy access to the client API, the several programmability APIs, the RESTFul management APIs and the Kubernetes resource definitions.

## APIs for Developers

### Client API

Redis comes with a wide range of commands that help you to develop real-time applications. You can find a complete overview of the Redis commands here:

[[commands|Redis commands]]

As a developer, you will likely use one of our supported client libraries for connecting and executing commands.

[[develop/clients|Connect with Redis clients introduction]]

### Programmability APIs

The existing Redis commands cover most use cases, but if low latency is a critical requirement, you might need to extend Redis' server-side functionality.

Lua scripts have been available since early versions of Redis. With Lua, the script is provided by the client and cached on the server side, which implies the risk that different clients might use a different script version.

[[develop/programmability/lua-api|Redis Lua API reference]]
[[develop/programmability/eval-intro|Scripting with Lua introduction]]

The Redis functions feature, which became available in Redis 7, supersedes the use of Lua in prior versions of Redis. The client is still responsible for invoking the execution, but unlike the previous Lua scripts, functions can now be replicated and persisted.

[[develop/programmability/functions-intro|Functions and scripting in Redis 7 and beyond]]

If none of the previous methods fulfills your needs, then you can extend the functionality of Redis with new commands using the Redis Modules API.

[Redis Modules API introduction](/docs/latest/develop/reference/modules/)
[Redis Modules API reference](/docs/latest/develop/reference/modules/modules-api-ref/)

## APIs for Operators

### Redis Cloud API

Redis Cloud is a fully managed Database as a Service offering and the fastest way to deploy Redis at scale. You can programmatically manage your databases, accounts, access, and credentials using the Redis Cloud REST API.

[[operate/rc/api|Redis Cloud REST API introduction]]
[Redis Cloud REST API examples](/docs/latest/operate/rc/api/examples/)
[[operate/rc/api/api-reference|Redis Cloud REST API reference]]

### Redis Software API

If you have installed Redis Software, you can automate operations with the Redis Software REST API.

[Redis Software REST API introduction](/docs/latest/operate/rs/references/rest-api/)
[Redis Software REST API requests](/docs/latest/operate/rs/references/rest-api/requests/)
[Redis Software REST API objects](/docs/latest/operate/rs/references/rest-api/objects/)

### Redis Enterprise for Kubernetes API

If you need to install Redis Enterprise on Kubernetes, then you can use the [Redis Enterprise for Kubernetes Operators](/docs/latest/operate/kubernetes/). You can find the resource definitions here:

[Redis Enterprise Cluster API](/docs/latest/operate/kubernetes/reference/api/redis_enterprise_cluster_api/)
[Redis Enterprise Database API](/docs/latest/operate/kubernetes/reference/api/redis_enterprise_database_api/)

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page