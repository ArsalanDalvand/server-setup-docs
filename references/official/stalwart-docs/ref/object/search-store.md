---
title: "SearchStore"
source: "https://stalw.art/docs/ref/object/search-store"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/search-store"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SearchStore"
---
# SearchStore

      

Configures the full-text search backend.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Storage › Search Store

## Fields

[Section titled “Fields”](#fields)

SearchStore is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Default"

[Section titled “@type: "Default"”](#type-default)

Use data store

### @type: "ElasticSearch"

[Section titled “@type: "ElasticSearch"”](#type-elasticsearch)

ElasticSearch

##### url

[Section titled “url”](#url)

Type: `Uri` · required

URL of the ElasticSearch server

##### numReplicas

[Section titled “numReplicas”](#numreplicas)

Type: `UnsignedInt` · default: `0` · max: 2048

Number of replicas for the index

##### numShards

[Section titled “numShards”](#numshards)

Type: `UnsignedInt` · default: `3` · max: 1048576 · min: 1

Number of shards for the index

##### includeSource

[Section titled “includeSource”](#includesource)

Type: `Boolean` · default: `false`

Whether to index the full source document

##### timeout

[Section titled “timeout”](#timeout)

Type: `Duration` · default: `"30s"`

Timeout for HTTP requests

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts)

Type: `Boolean` · default: `false`

Whether to allow invalid SSL certificates

##### httpAuth

[Section titled “httpAuth”](#httpauth)

Type: [`HttpAuth`](#httpauth) · required

The type of HTTP authentication to use

##### httpHeaders

[Section titled “httpHeaders”](#httpheaders)

Type: `Map<String, String>`

Additional headers to include in HTTP requests

### @type: "Meilisearch"

[Section titled “@type: "Meilisearch"”](#type-meilisearch)

MeiliSearch

##### url

[Section titled “url”](#url-1)

Type: `Uri` · required

URL of the store

##### pollInterval

[Section titled “pollInterval”](#pollinterval)

Type: `Duration` · default: `"500ms"`

Interval between polling for task status

##### maxRetries

[Section titled “maxRetries”](#maxretries)

Type: `UnsignedInt` · default: `120` · max: 1024 · min: 1

Number of times to poll for task status before giving up

##### failOnTimeout

[Section titled “failOnTimeout”](#failontimeout)

Type: `Boolean` · default: `true`

Whether to fail the operation if the task does not complete within the polling retries

##### timeout

[Section titled “timeout”](#timeout-1)

Type: `Duration` · default: `"30s"`

Timeout for HTTP requests

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts-1)

Type: `Boolean` · default: `false`

Whether to allow invalid SSL certificates

##### httpAuth

[Section titled “httpAuth”](#httpauth-1)

Type: [`HttpAuth`](#httpauth) · required

The type of HTTP authentication to use

##### httpHeaders

[Section titled “httpHeaders”](#httpheaders-1)

Type: `Map<String, String>`

Additional headers to include in HTTP requests

### @type: "FoundationDb"

[Section titled “@type: "FoundationDb"”](#type-foundationdb)

FoundationDB

##### clusterFile

[Section titled “clusterFile”](#clusterfile)

Type: `String?`

Path to the cluster file for the FoundationDB cluster

##### datacenterId

[Section titled “datacenterId”](#datacenterid)

Type: `String?`

Data center ID (optional)

##### machineId

[Section titled “machineId”](#machineid)

Type: `String?`

Machine ID in the FoundationDB cluster (optional)

##### transactionRetryDelay

[Section titled “transactionRetryDelay”](#transactionretrydelay)

Type: `Duration?`

Transaction maximum retry delay

##### transactionRetryLimit

[Section titled “transactionRetryLimit”](#transactionretrylimit)

Type: `UnsignedInt?` · max: 1000 · min: 1

Transaction retry limit

##### transactionTimeout

[Section titled “transactionTimeout”](#transactiontimeout)

Type: `Duration?`

Transaction timeout

### @type: "PostgreSql"

[Section titled “@type: "PostgreSql"”](#type-postgresql)

PostgreSQL

##### timeout

[Section titled “timeout”](#timeout-2)

Type: `Duration?` · default: `"15s"`

Connection timeout to the database

##### useTls

[Section titled “useTls”](#usetls)

Type: `Boolean` · default: `false`

Use TLS to connect to the store

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts-2)

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

### @type: "MySql"

[Section titled “@type: "MySql"”](#type-mysql)

mySQL

##### timeout

[Section titled “timeout”](#timeout-3)

Type: `Duration?` · default: `"15s"`

Connection timeout to the database

##### useTls

[Section titled “useTls”](#usetls-1)

Type: `Boolean` · default: `false`

Use TLS to connect to the store

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts-3)

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

[Section titled “host”](#host-1)

Type: `String` · required

Hostname of the database server

##### port

[Section titled “port”](#port-1)

Type: `UnsignedInt` · default: `3306` · max: 65535 · min: 1

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

## JMAP API

[Section titled “JMAP API”](#jmap-api)

The SearchStore singleton is available via the `urn:stalwart:jmap` capability.

### x:SearchStore/get

[Section titled “x:SearchStore/get”](#xsearchstoreget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysSearchStoreGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SearchStore/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:SearchStore/set

[Section titled “x:SearchStore/set”](#xsearchstoreset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysSearchStoreUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:SearchStore/set",          {            "update": {              "singleton": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get SearchStore
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update SearchStore --field description='updated value'
```

## Nested types

[Section titled “Nested types”](#nested-types)

### HttpAuth

[Section titled “HttpAuth”](#httpauth-2)

Defines the HTTP authentication method to use for HTTP requests.

`Unauthenticated`: Anonymous. No additional fields.
`Basic`: Basic Authentication. Carries the fields of [`HttpAuthBasic`](#httpauthbasic).
`Bearer`: Bearer Token. Carries the fields of [`HttpAuthBearer`](#httpauthbearer).

#### HttpAuthBasic

[Section titled “HttpAuthBasic”](#httpauthbasic)

HTTP Basic authentication credentials.

##### username

[Section titled “username”](#username)

Type: `String` · required

Username for HTTP Basic Authentication

##### secret

[Section titled “secret”](#secret)

Type: [`SecretKey`](#secretkey) · required

Password for HTTP Basic Authentication

##### SecretKey

[Section titled “SecretKey”](#secretkey)

A secret value provided directly, from an environment variable, or from a file.

`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

##### SecretKeyValue

[Section titled “SecretKeyValue”](#secretkeyvalue)

A secret value provided directly.

##### secret

[Section titled “secret”](#secret-1)

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

#### HttpAuthBearer

[Section titled “HttpAuthBearer”](#httpauthbearer)

HTTP Bearer token authentication.

##### bearerToken

[Section titled “bearerToken”](#bearertoken)

Type: [`SecretKey`](#secretkey) · required

Bearer token for HTTP Bearer Authentication

### PostgreSqlSettings

[Section titled “PostgreSqlSettings”](#postgresqlsettings)

PostgreSQL connection settings.

##### host

[Section titled “host”](#host-2)

Type: `String` · required

Hostname of the database server

##### port

[Section titled “port”](#port-2)

Type: `UnsignedInt` · default: `5432` · max: 65535 · min: 1

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

##### options

[Section titled “options”](#options-1)

Type: `String?`

Additional connection options

#### SecretKeyOptional

[Section titled “SecretKeyOptional”](#secretkeyoptional)

An optional secret value, or none.

`None`: No secret. No additional fields.
`Value`: Secret value. Carries the fields of [`SecretKeyValue`](#secretkeyvalue).
`EnvironmentVariable`: Secret read from environment variable. Carries the fields of [`SecretKeyEnvironmentVariable`](#secretkeyenvironmentvariable).
`File`: Secret read from file. Carries the fields of [`SecretKeyFile`](#secretkeyfile).

### MySqlSettings

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

## Enums

[Section titled “Enums”](#enums)

### PostgreSqlRecyclingMethod

[Section titled “PostgreSqlRecyclingMethod”](#postgresqlrecyclingmethod)
ValueLabel`fast`Fast recycling method`verified`Verified recycling method`clean`Clean recycling method