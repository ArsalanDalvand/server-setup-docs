---
title: "InMemoryStore"
source: "https://stalw.art/docs/ref/object/in-memory-store"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/in-memory-store"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "InMemoryStore"
---
# InMemoryStore

      

Configures the in-memory cache and lookup store.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Storage › In-Memory Store

## Fields

[Section titled “Fields”](#fields)

InMemoryStore is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Default"

[Section titled “@type: "Default"”](#type-default)

Use data store

### @type: "Sharded"

[Section titled “@type: "Sharded"”](#type-sharded)

Sharded Store

##### stores

[Section titled “stores”](#stores)

Type: [`InMemoryStoreBase`](#inmemorystorebase)`[]` · min items: 2

Stores to use for sharding

### @type: "Redis"

[Section titled “@type: "Redis"”](#type-redis)

Redis/Valkey

##### url

[Section titled “url”](#url)

Type: `Uri` · default: `"redis://127.0.0.1"`

URL of the Redis server

##### timeout

[Section titled “timeout”](#timeout)

Type: `Duration` · default: `"10s"`

Connection timeout to the database

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections)

Type: `UnsignedInt` · default: `10` · max: 8192 · min: 1

Maximum number of connections to the store

##### poolTimeoutCreate

[Section titled “poolTimeoutCreate”](#pooltimeoutcreate)

Type: `Duration?` · default: `"30s"`

Timeout for creating a new connection

##### poolTimeoutWait

[Section titled “poolTimeoutWait”](#pooltimeoutwait)

Type: `Duration?` · default: `"30s"`

Timeout for waiting for a connection from the pool

##### poolTimeoutRecycle

[Section titled “poolTimeoutRecycle”](#pooltimeoutrecycle)

Type: `Duration?` · default: `"30s"`

Timeout for recycling a connection

### @type: "RedisCluster"

[Section titled “@type: "RedisCluster"”](#type-rediscluster)

Redis Cluster

##### urls

[Section titled “urls”](#urls)

Type: `Uri[]` · default: `["redis://127.0.0.1"]`

URL(s) of the Redis server(s)

##### timeout

[Section titled “timeout”](#timeout-1)

Type: `Duration` · default: `"10s"`

Connection timeout to the database

##### authUsername

[Section titled “authUsername”](#authusername)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the store

##### maxRetryWait

[Section titled “maxRetryWait”](#maxretrywait)

Type: `Duration?` · max: 1024 · min: 1

Maximum time to wait between retries

##### minRetryWait

[Section titled “minRetryWait”](#minretrywait)

Type: `Duration?` · max: 1024 · min: 1

Minimum time to wait between retries

##### maxRetries

[Section titled “maxRetries”](#maxretries)

Type: `UnsignedInt?` · max: 1024 · min: 1

Number of retries to connect to the Redis cluster

##### readFromReplicas

[Section titled “readFromReplicas”](#readfromreplicas)

Type: `Boolean` · default: `true`

Whether to read from replicas

##### protocolVersion

[Section titled “protocolVersion”](#protocolversion)

Type: [`RedisProtocol`](#redisprotocol) · default: `"resp2"`

Protocol Version

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections-1)

Type: `UnsignedInt` · default: `10` · max: 8192 · min: 1

Maximum number of connections to the store

##### poolTimeoutCreate

[Section titled “poolTimeoutCreate”](#pooltimeoutcreate-1)

Type: `Duration?` · default: `"30s"`

Timeout for creating a new connection

##### poolTimeoutWait

[Section titled “poolTimeoutWait”](#pooltimeoutwait-1)

Type: `Duration?` · default: `"30s"`

Timeout for waiting for a connection from the pool

##### poolTimeoutRecycle

[Section titled “poolTimeoutRecycle”](#pooltimeoutrecycle-1)

Type: `Duration?` · default: `"30s"`

Timeout for recycling a connection

### @type: "RedisSentinel"

[Section titled “@type: "RedisSentinel"”](#type-redissentinel)

Redis Sentinel

##### urls

[Section titled “urls”](#urls-1)

Type: `Uri[]` · default: `["redis://127.0.0.1:26379"]`

Address(es) of the Sentinel node(s)

##### serviceName

[Section titled “serviceName”](#servicename)

Type: `String` · default: `"mymaster"`

Name of the monitored master (service) to query via the Sentinels

##### timeout

[Section titled “timeout”](#timeout-2)

Type: `Duration` · default: `"10s"`

Connection timeout to the database

##### authUsername

[Section titled “authUsername”](#authusername-1)

Type: `String?` · default: `"stalwart"`

Username to connect to the data nodes

##### authSecret

[Section titled “authSecret”](#authsecret-1)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the data nodes

##### sentinelUsername

[Section titled “sentinelUsername”](#sentinelusername)

Type: `String?`

Username to connect to the Sentinel nodes

##### sentinelSecret

[Section titled “sentinelSecret”](#sentinelsecret)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the Sentinel nodes

##### protocolVersion

[Section titled “protocolVersion”](#protocolversion-1)

Type: [`RedisProtocol`](#redisprotocol) · default: `"resp2"`

Protocol Version

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections-2)

Type: `UnsignedInt` · default: `10` · max: 8192 · min: 1

Maximum number of connections to the store

##### poolTimeoutCreate

[Section titled “poolTimeoutCreate”](#pooltimeoutcreate-2)

Type: `Duration?` · default: `"30s"`

Timeout for creating a new connection

##### poolTimeoutWait

[Section titled “poolTimeoutWait”](#pooltimeoutwait-2)

Type: `Duration?` · default: `"30s"`

Timeout for waiting for a connection from the pool

##### poolTimeoutRecycle

[Section titled “poolTimeoutRecycle”](#pooltimeoutrecycle-2)

Type: `Duration?` · default: `"30s"`

Timeout for recycling a connection

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The InMemoryStore singleton is available via the `urn:stalwart:jmap` capability.

### x:InMemoryStore/get

[Section titled “x:InMemoryStore/get”](#xinmemorystoreget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysInMemoryStoreGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:InMemoryStore/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:InMemoryStore/set

[Section titled “x:InMemoryStore/set”](#xinmemorystoreset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysInMemoryStoreUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:InMemoryStore/set",          {            "update": {              "singleton": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get InMemoryStore
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update InMemoryStore --field description='updated value'
```

## Nested types

[Section titled “Nested types”](#nested-types)

### InMemoryStoreBase

[Section titled “InMemoryStoreBase”](#inmemorystorebase)

In-memory store backends.

`Redis`: Redis/Valkey. Carries the fields of [`RedisStore`](#redisstore).
`RedisCluster`: Redis Cluster. Carries the fields of [`RedisClusterStore`](#redisclusterstore).
`RedisSentinel`: Redis Sentinel. Carries the fields of [`RedisSentinelStore`](#redissentinelstore).

#### RedisStore

[Section titled “RedisStore”](#redisstore)

Redis/Valkey store.

##### url

[Section titled “url”](#url-1)

Type: `Uri` · default: `"redis://127.0.0.1"`

URL of the Redis server

##### timeout

[Section titled “timeout”](#timeout-3)

Type: `Duration` · default: `"10s"`

Connection timeout to the database

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections-3)

Type: `UnsignedInt` · default: `10` · max: 8192 · min: 1

Maximum number of connections to the store

##### poolTimeoutCreate

[Section titled “poolTimeoutCreate”](#pooltimeoutcreate-3)

Type: `Duration?` · default: `"30s"`

Timeout for creating a new connection

##### poolTimeoutWait

[Section titled “poolTimeoutWait”](#pooltimeoutwait-3)

Type: `Duration?` · default: `"30s"`

Timeout for waiting for a connection from the pool

##### poolTimeoutRecycle

[Section titled “poolTimeoutRecycle”](#pooltimeoutrecycle-3)

Type: `Duration?` · default: `"30s"`

Timeout for recycling a connection

#### RedisClusterStore

[Section titled “RedisClusterStore”](#redisclusterstore)

Redis Cluster store.

##### urls

[Section titled “urls”](#urls-2)

Type: `Uri[]` · default: `["redis://127.0.0.1"]`

URL(s) of the Redis server(s)

##### timeout

[Section titled “timeout”](#timeout-4)

Type: `Duration` · default: `"10s"`

Connection timeout to the database

##### authUsername

[Section titled “authUsername”](#authusername-2)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret-2)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the store

##### maxRetryWait

[Section titled “maxRetryWait”](#maxretrywait-1)

Type: `Duration?` · max: 1024 · min: 1

Maximum time to wait between retries

##### minRetryWait

[Section titled “minRetryWait”](#minretrywait-1)

Type: `Duration?` · max: 1024 · min: 1

Minimum time to wait between retries

##### maxRetries

[Section titled “maxRetries”](#maxretries-1)

Type: `UnsignedInt?` · max: 1024 · min: 1

Number of retries to connect to the Redis cluster

##### readFromReplicas

[Section titled “readFromReplicas”](#readfromreplicas-1)

Type: `Boolean` · default: `true`

Whether to read from replicas

##### protocolVersion

[Section titled “protocolVersion”](#protocolversion-2)

Type: [`RedisProtocol`](#redisprotocol) · default: `"resp2"`

Protocol Version

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections-4)

Type: `UnsignedInt` · default: `10` · max: 8192 · min: 1

Maximum number of connections to the store

##### poolTimeoutCreate

[Section titled “poolTimeoutCreate”](#pooltimeoutcreate-4)

Type: `Duration?` · default: `"30s"`

Timeout for creating a new connection

##### poolTimeoutWait

[Section titled “poolTimeoutWait”](#pooltimeoutwait-4)

Type: `Duration?` · default: `"30s"`

Timeout for waiting for a connection from the pool

##### poolTimeoutRecycle

[Section titled “poolTimeoutRecycle”](#pooltimeoutrecycle-4)

Type: `Duration?` · default: `"30s"`

Timeout for recycling a connection

##### SecretKeyOptional

[Section titled “SecretKeyOptional”](#secretkeyoptional)

An optional secret value, or none.

`None`: No secret. No additional fields.
`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

##### SecretKeyValue

[Section titled “SecretKeyValue”](#secretkeyvalue)

A secret value provided directly.

##### secret

[Section titled “secret”](#secret)

Type: `String` · required · secret

Password or secret value

##### SecretKeyEnvironmentVariable

[Section titled “SecretKeyEnvironmentVariable”](#secretkeyenvironmentvariable)

A secret value read from an environment variable.

##### variableName

[Section titled “variableName”](#variablename)

Type: `String` · required

Environment variable name to read the secret from

##### SecretKeyFile

[Section titled “SecretKeyFile”](#secretkeyfile)

A secret value read from a file.

##### filePath

[Section titled “filePath”](#filepath)

Type: `String` · required

File path to read the secret from

#### RedisSentinelStore

[Section titled “RedisSentinelStore”](#redissentinelstore)

Redis Sentinel store.

##### urls

[Section titled “urls”](#urls-3)

Type: `Uri[]` · default: `["redis://127.0.0.1:26379"]`

Address(es) of the Sentinel node(s)

##### serviceName

[Section titled “serviceName”](#servicename-1)

Type: `String` · default: `"mymaster"`

Name of the monitored master (service) to query via the Sentinels

##### timeout

[Section titled “timeout”](#timeout-5)

Type: `Duration` · default: `"10s"`

Connection timeout to the database

##### authUsername

[Section titled “authUsername”](#authusername-3)

Type: `String?` · default: `"stalwart"`

Username to connect to the data nodes

##### authSecret

[Section titled “authSecret”](#authsecret-3)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the data nodes

##### sentinelUsername

[Section titled “sentinelUsername”](#sentinelusername-1)

Type: `String?`

Username to connect to the Sentinel nodes

##### sentinelSecret

[Section titled “sentinelSecret”](#sentinelsecret-1)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the Sentinel nodes

##### protocolVersion

[Section titled “protocolVersion”](#protocolversion-3)

Type: [`RedisProtocol`](#redisprotocol) · default: `"resp2"`

Protocol Version

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections-5)

Type: `UnsignedInt` · default: `10` · max: 8192 · min: 1

Maximum number of connections to the store

##### poolTimeoutCreate

[Section titled “poolTimeoutCreate”](#pooltimeoutcreate-5)

Type: `Duration?` · default: `"30s"`

Timeout for creating a new connection

##### poolTimeoutWait

[Section titled “poolTimeoutWait”](#pooltimeoutwait-5)

Type: `Duration?` · default: `"30s"`

Timeout for waiting for a connection from the pool

##### poolTimeoutRecycle

[Section titled “poolTimeoutRecycle”](#pooltimeoutrecycle-5)

Type: `Duration?` · default: `"30s"`

Timeout for recycling a connection

## Enums

[Section titled “Enums”](#enums)

### RedisProtocol

[Section titled “RedisProtocol”](#redisprotocol)
ValueLabel`resp2`RESP2`resp3`RESP3