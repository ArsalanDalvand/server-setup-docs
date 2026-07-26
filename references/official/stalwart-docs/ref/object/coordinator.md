---
title: "Coordinator"
source: "https://stalw.art/docs/ref/object/coordinator"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/coordinator"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Coordinator"
---
# Coordinator

      

Configures the cluster coordinator for inter-node communication.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Cluster › Coordinator

## Fields

[Section titled “Fields”](#fields)

Coordinator is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Disabled"

[Section titled “@type: "Disabled"”](#type-disabled)

Disabled

### @type: "Default"

[Section titled “@type: "Default"”](#type-default)

Use in-memory store (Redis only)

### @type: "Kafka"

[Section titled “@type: "Kafka"”](#type-kafka)

Kafka

##### brokers

[Section titled “brokers”](#brokers)

Type: `String[]` · min items: 1

List of Kafka brokers

##### groupId

[Section titled “groupId”](#groupid)

Type: `String` · required

Consumer group ID

##### timeoutMessage

[Section titled “timeoutMessage”](#timeoutmessage)

Type: `Duration` · default: `"5s"`

Timeout for message processing

##### timeoutSession

[Section titled “timeoutSession”](#timeoutsession)

Type: `Duration` · default: `"5s"`

Timeout for session

### @type: "Nats"

[Section titled “@type: "Nats"”](#type-nats)

NATS

##### addresses

[Section titled “addresses”](#addresses)

Type: `String[]` · default: `["127.0.0.1:4444"]` · min items: 1

Address of the NATS server

##### maxReconnects

[Section titled “maxReconnects”](#maxreconnects)

Type: `UnsignedInt?`

Maximum number of times to attempt to reconnect to the server

##### timeoutConnection

[Section titled “timeoutConnection”](#timeoutconnection)

Type: `Duration` · default: `"5s"`

Timeout for establishing a connection to the server

##### timeoutRequest

[Section titled “timeoutRequest”](#timeoutrequest)

Type: `Duration` · default: `"10s"`

Timeout for requests to the server

##### pingInterval

[Section titled “pingInterval”](#pinginterval)

Type: `Duration` · default: `"60s"`

Interval between pings to the server

##### capacityClient

[Section titled “capacityClient”](#capacityclient)

Type: `UnsignedInt` · default: `2048` · min: 1

By default, Client dispatches op’s to the Client onto the channel with capacity of 2048. This option enables overriding it

##### capacityReadBuffer

[Section titled “capacityReadBuffer”](#capacityreadbuffer)

Type: `UnsignedInt` · default: `65535` · min: 1

Sets the initial capacity of the read buffer. Which is a buffer used to gather partial protocol messages.

##### capacitySubscription

[Section titled “capacitySubscription”](#capacitysubscription)

Type: `UnsignedInt` · default: `65536` · min: 1

Sets the capacity for Subscribers. Exceeding it will trigger slow consumer error callback and drop messages.

##### noEcho

[Section titled “noEcho”](#noecho)

Type: `Boolean` · default: `true`

Disables delivering messages that were published from the same connection.

##### useTls

[Section titled “useTls”](#usetls)

Type: `Boolean` · default: `false`

Use TLS to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the store

##### authUsername

[Section titled “authUsername”](#authusername)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### credentials

[Section titled “credentials”](#credentials)

Type: [`SecretTextOptional`](#secrettextoptional) · required

String containing the JWT credentials

### @type: "Zenoh"

[Section titled “@type: "Zenoh"”](#type-zenoh)

Eclipse Zenoh

##### config

[Section titled “config”](#config)

Type: `Text` · required

Zenoh configuration string

### @type: "Redis"

[Section titled “@type: "Redis"”](#type-redis)

Redis

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

[Section titled “authUsername”](#authusername-1)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret-1)

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

[Section titled “authUsername”](#authusername-2)

Type: `String?` · default: `"stalwart"`

Username to connect to the data nodes

##### authSecret

[Section titled “authSecret”](#authsecret-2)

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

The Coordinator singleton is available via the `urn:stalwart:jmap` capability.

### x:Coordinator/get

[Section titled “x:Coordinator/get”](#xcoordinatorget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysCoordinatorGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Coordinator/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:Coordinator/set

[Section titled “x:Coordinator/set”](#xcoordinatorset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysCoordinatorUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:Coordinator/set",          {            "update": {              "singleton": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get Coordinator
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update Coordinator --field description='updated value'
```

## Nested types

[Section titled “Nested types”](#nested-types)

### SecretKeyOptional

[Section titled “SecretKeyOptional”](#secretkeyoptional)

An optional secret value, or none.

`None`: No secret. No additional fields.
`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

#### SecretKeyValue

[Section titled “SecretKeyValue”](#secretkeyvalue)

A secret value provided directly.

##### secret

[Section titled “secret”](#secret)

Type: `String` · required · secret

Password or secret value

#### SecretKeyEnvironmentVariable

[Section titled “SecretKeyEnvironmentVariable”](#secretkeyenvironmentvariable)

A secret value read from an environment variable.

##### variableName

[Section titled “variableName”](#variablename)

Type: `String` · required

Environment variable name to read the secret from

#### SecretKeyFile

[Section titled “SecretKeyFile”](#secretkeyfile)

A secret value read from a file.

##### filePath

[Section titled “filePath”](#filepath)

Type: `String` · required

File path to read the secret from

### SecretTextOptional

[Section titled “SecretTextOptional”](#secrettextoptional)

An optional secret text value, or none.

`None`: No secret. No additional fields.
`Text`: Secret value. Carries the fields of [`SecretTextValue`](#secrettextvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

#### SecretTextValue

[Section titled “SecretTextValue”](#secrettextvalue)

A secret text value provided directly.

##### secret

[Section titled “secret”](#secret-1)

Type: `Text` · required · secret

Password or secret value

## Enums

[Section titled “Enums”](#enums)

### RedisProtocol

[Section titled “RedisProtocol”](#redisprotocol)
ValueLabel`resp2`RESP2`resp3`RESP3