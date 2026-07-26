---
title: "StoreLookup"
source: "https://stalw.art/docs/ref/object/store-lookup"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/store-lookup"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "StoreLookup"
---
# StoreLookup

      

Defines an external store used for lookups.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Lookups › Store Lookups

## Fields

[Section titled “Fields”](#fields)

##### namespace

[Section titled “namespace”](#namespace)

Type: `String` · read-only

Unique identifier for this store when used in lookups

##### store

[Section titled “store”](#store)

Type: [`LookupStore`](#lookupstore) · required

Store to use for lookups

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The StoreLookup object is available via the `urn:stalwart:jmap` capability.

### x:StoreLookup/get

[Section titled “x:StoreLookup/get”](#xstorelookupget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

This method requires the `sysStoreLookupGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:StoreLookup/get",          {            "ids": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:StoreLookup/set

[Section titled “x:StoreLookup/set”](#xstorelookupset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

Supports create, update, and destroy operations in a single call.

#### Create

[Section titled “Create”](#create)

This operation requires the `sysStoreLookupCreate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:StoreLookup/set",          {            "create": {              "new1": {                "store": {                  "@type": "PostgreSql",                  "authSecret": {                    "@type": "None"                  },                  "host": "Example",                  "readReplicas": {}                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Update

[Section titled “Update”](#update)

This operation requires the `sysStoreLookupUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:StoreLookup/set",          {            "update": {              "id1": {                "store": {                  "@type": "PostgreSql",                  "authSecret": {                    "@type": "None"                  },                  "host": "Example",                  "readReplicas": {}                }              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

#### Destroy

[Section titled “Destroy”](#destroy)

This operation requires the `sysStoreLookupDestroy` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:StoreLookup/set",          {            "destroy": [              "id1"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:StoreLookup/query

[Section titled “x:StoreLookup/query”](#xstorelookupquery)

This is a standard [`Foo/query`](https://www.rfc-editor.org/rfc/rfc8620#section-5.5) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.5), Section 5.5.

This method requires the `sysStoreLookupQuery` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:StoreLookup/query",          {            "filter": {}          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get StoreLookup id1
```

### Create

[Section titled “Create”](#create-1)
Terminal window

```
stalwart-cli create StoreLookup \  --field 'store={"@type":"PostgreSql","authSecret":{"@type":"None"},"host":"Example","readReplicas":{}}'
```

### Query

[Section titled “Query”](#query)
Terminal window

```
stalwart-cli query StoreLookup
```

### Update

[Section titled “Update”](#update-1)
Terminal window

```
stalwart-cli update StoreLookup id1 --field store='{"@type":"PostgreSql","authSecret":{"@type":"None"},"host":"Example","readReplicas":{}}'
```

### Delete

[Section titled “Delete”](#delete)
Terminal window

```
stalwart-cli delete StoreLookup --ids id1
```

## Nested types

[Section titled “Nested types”](#nested-types)

### LookupStore

[Section titled “LookupStore”](#lookupstore)

Lookup store backends.

`PostgreSql`: PostgreSQL. Carries the fields of [`PostgreSqlStore`](#postgresqlstore).
`MySql`: mySQL. Carries the fields of [`MySqlStore`](#mysqlstore).
`Sqlite`: SQLite. Carries the fields of [`SqliteStore`](#sqlitestore).
`Sharded`: Sharded Lookup Store. Carries the fields of [`ShardedInMemoryStore`](#shardedinmemorystore).
`Redis`: Redis/Valkey. Carries the fields of [`RedisStore`](#redisstore).
`RedisCluster`: Redis Cluster. Carries the fields of [`RedisClusterStore`](#redisclusterstore).
`RedisSentinel`: Redis Sentinel. Carries the fields of [`RedisSentinelStore`](#redissentinelstore).

#### PostgreSqlStore

[Section titled “PostgreSqlStore”](#postgresqlstore)

PostgreSQL data store.

##### timeout

[Section titled “timeout”](#timeout)

Type: `Duration?` · default: `"15s"`

Connection timeout to the database

##### useTls

[Section titled “useTls”](#usetls)

Type: `Boolean` · default: `false`

Use TLS to connect to the store

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts)

Type: `Boolean` · default: `false`

Allow invalid TLS certificates when connecting to the store

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections)

Type: `UnsignedInt?` · default: `10` · max: 8192 · min: 1

Maximum number of connections to the store

##### poolRecyclingMethod

[Section titled “poolRecyclingMethod”](#poolrecyclingmethod)

Type: [`PostgreSqlRecyclingMethod`](#postgresqlrecyclingmethod) · default: `"fast"`

Method to use when recycling connections in the pool

##### readReplicas

[Section titled “readReplicas”](#readreplicas)

Type: [`PostgreSqlSettings`](#postgresqlsettings)`[]` · [[server/enterprise|enterprise]]

List of read replicas for the store

##### host

[Section titled “host”](#host)

Type: `String` · required

Hostname of the database server

##### port

[Section titled “port”](#port)

Type: `UnsignedInt` · default: `5432` · max: 65535 · min: 1

Port of the database server

##### database

[Section titled “database”](#database)

Type: `String` · default: `"stalwart"`

Name of the database

##### authUsername

[Section titled “authUsername”](#authusername)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the store

##### options

[Section titled “options”](#options)

Type: `String?`

Additional connection options

##### PostgreSqlSettings

[Section titled “PostgreSqlSettings”](#postgresqlsettings)

PostgreSQL connection settings.

##### host

[Section titled “host”](#host-1)

Type: `String` · required

Hostname of the database server

##### port

[Section titled “port”](#port-1)

Type: `UnsignedInt` · default: `5432` · max: 65535 · min: 1

Port of the database server

##### database

[Section titled “database”](#database-1)

Type: `String` · default: `"stalwart"`

Name of the database

##### authUsername

[Section titled “authUsername”](#authusername-1)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret-1)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the store

##### options

[Section titled “options”](#options-1)

Type: `String?`

Additional connection options

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

#### MySqlStore

[Section titled “MySqlStore”](#mysqlstore)

MySQL data store.

##### timeout

[Section titled “timeout”](#timeout-1)

Type: `Duration?` · default: `"15s"`

Connection timeout to the database

##### useTls

[Section titled “useTls”](#usetls-1)

Type: `Boolean` · default: `false`

Use TLS to connect to the store

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts-1)

Type: `Boolean` · default: `false`

Allow invalid TLS certificates when connecting to the store

##### maxAllowedPacket

[Section titled “maxAllowedPacket”](#maxallowedpacket)

Type: `UnsignedInt?` · max: 1073741824 · min: 1024

Maximum size of a packet in bytes

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections-1)

Type: `UnsignedInt?` · default: `10` · max: 8192 · min: 1

Maximum number of connections to the store

##### poolMinConnections

[Section titled “poolMinConnections”](#poolminconnections)

Type: `UnsignedInt?` · default: `5` · max: 8192 · min: 1

Minimum number of connections to the store

##### readReplicas

[Section titled “readReplicas”](#readreplicas-1)

Type: [`MySqlSettings`](#mysqlsettings)`[]` · [[server/enterprise|enterprise]]

List of read replicas for the store

##### host

[Section titled “host”](#host-2)

Type: `String` · required

Hostname of the database server

##### port

[Section titled “port”](#port-2)

Type: `UnsignedInt` · default: `3306` · max: 65535 · min: 1

Port of the database server

##### database

[Section titled “database”](#database-2)

Type: `String` · default: `"stalwart"`

Name of the database

##### authUsername

[Section titled “authUsername”](#authusername-2)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret-2)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the store

##### MySqlSettings

[Section titled “MySqlSettings”](#mysqlsettings)

MySQL connection settings.

##### host

[Section titled “host”](#host-3)

Type: `String` · required

Hostname of the database server

##### port

[Section titled “port”](#port-3)

Type: `UnsignedInt` · default: `3306` · max: 65535 · min: 1

Port of the database server

##### database

[Section titled “database”](#database-3)

Type: `String` · default: `"stalwart"`

Name of the database

##### authUsername

[Section titled “authUsername”](#authusername-3)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret-3)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the store

#### SqliteStore

[Section titled “SqliteStore”](#sqlitestore)

SQLite embedded data store.

##### path

[Section titled “path”](#path)

Type: `String` · required

Path to the SQLite data directory

##### poolWorkers

[Section titled “poolWorkers”](#poolworkers)

Type: `UnsignedInt?` · max: 64 · min: 1

Number of worker threads to use for the store, defaults to the number of cores

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections-2)

Type: `UnsignedInt` · default: `10` · max: 8192 · min: 1

Maximum number of connections to the store

#### ShardedInMemoryStore

[Section titled “ShardedInMemoryStore”](#shardedinmemorystore)

Sharded in-memory store configuration.

##### stores

[Section titled “stores”](#stores)

Type: [`InMemoryStoreBase`](#inmemorystorebase)`[]` · min items: 2

Stores to use for sharding

##### InMemoryStoreBase

[Section titled “InMemoryStoreBase”](#inmemorystorebase)

In-memory store backends.

`Redis`: Redis/Valkey. Carries the fields of [`RedisStore`](#redisstore).
`RedisCluster`: Redis Cluster. Carries the fields of [`RedisClusterStore`](#redisclusterstore).
`RedisSentinel`: Redis Sentinel. Carries the fields of [`RedisSentinelStore`](#redissentinelstore).

##### RedisStore

[Section titled “RedisStore”](#redisstore)

Redis/Valkey store.

##### url

[Section titled “url”](#url)

Type: `Uri` · default: `"redis://127.0.0.1"`

URL of the Redis server

##### timeout

[Section titled “timeout”](#timeout-2)

Type: `Duration` · default: `"10s"`

Connection timeout to the database

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections-3)

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

##### RedisClusterStore

[Section titled “RedisClusterStore”](#redisclusterstore)

Redis Cluster store.

##### urls

[Section titled “urls”](#urls)

Type: `Uri[]` · default: `["redis://127.0.0.1"]`

URL(s) of the Redis server(s)

##### timeout

[Section titled “timeout”](#timeout-3)

Type: `Duration` · default: `"10s"`

Connection timeout to the database

##### authUsername

[Section titled “authUsername”](#authusername-4)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret-4)

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

[Section titled “poolMaxConnections”](#poolmaxconnections-4)

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

##### RedisSentinelStore

[Section titled “RedisSentinelStore”](#redissentinelstore)

Redis Sentinel store.

##### urls

[Section titled “urls”](#urls-1)

Type: `Uri[]` · default: `["redis://127.0.0.1:26379"]`

Address(es) of the Sentinel node(s)

##### serviceName

[Section titled “serviceName”](#servicename)

Type: `String` · default: `"mymaster"`

Name of the monitored master (service) to query via the Sentinels

##### timeout

[Section titled “timeout”](#timeout-4)

Type: `Duration` · default: `"10s"`

Connection timeout to the database

##### authUsername

[Section titled “authUsername”](#authusername-5)

Type: `String?` · default: `"stalwart"`

Username to connect to the data nodes

##### authSecret

[Section titled “authSecret”](#authsecret-5)

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

[Section titled “poolMaxConnections”](#poolmaxconnections-5)

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

## Enums

[Section titled “Enums”](#enums)

### PostgreSqlRecyclingMethod

[Section titled “PostgreSqlRecyclingMethod”](#postgresqlrecyclingmethod)
ValueLabel`fast`Fast recycling method`verified`Verified recycling method`clean`Clean recycling method

### RedisProtocol

[Section titled “RedisProtocol”](#redisprotocol)
ValueLabel`resp2`RESP2`resp3`RESP3