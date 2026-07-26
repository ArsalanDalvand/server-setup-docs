---
title: "BlobStore"
source: "https://stalw.art/docs/ref/object/blob-store"
source_format: official-html-to-markdown
language: "en"
doc_slug: "ref/object/blob-store"
category: "ref"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "BlobStore"
---
# BlobStore

      

Configures the blob storage backend for messages and files.

This object can be configured from the [[management/webui|WebUI]] under  Settings ›  Storage › Blob Store

## Fields

[Section titled “Fields”](#fields)

BlobStore is a multi-variant object: each instance has an `@type` discriminator selecting one of the variants below, and each variant carries its own set of fields.

### @type: "Default"

[Section titled “@type: "Default"”](#type-default)

Use data store

### @type: "Sharded"

[Section titled “@type: "Sharded"”](#type-sharded)

Sharded Blob Store

##### stores

[Section titled “stores”](#stores)

Type: [`BlobStoreBase`](#blobstorebase)`[]` · min items: 2

Stores to use for sharding

### @type: "S3"

[Section titled “@type: "S3"”](#type-s3)

S3-compatible

##### region

[Section titled “region”](#region)

Type: [`S3StoreRegion`](#s3storeregion) · required

The S3 region where the bucket resides

##### bucket

[Section titled “bucket”](#bucket)

Type: `String` · required

The S3 bucket where blobs (e-mail messages, Sieve scripts, etc.) will be stored

##### accessKey

[Section titled “accessKey”](#accesskey)

Type: `String?`

Identifies the S3 account

##### secretKey

[Section titled “secretKey”](#secretkey)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

The secret key for the S3 account

##### securityToken

[Section titled “securityToken”](#securitytoken)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Security token for temporary credentials

##### sessionToken

[Section titled “sessionToken”](#sessiontoken)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Temporary session token for the S3 account

##### profile

[Section titled “profile”](#profile)

Type: `String?`

Used when retrieving credentials from a shared credentials file. If specified, the server will use the access key ID, secret access key, and session token (if available) associated with the given profile

##### timeout

[Section titled “timeout”](#timeout)

Type: `Duration` · default: `"30s"`

Connection timeout to the S3 service

##### maxRetries

[Section titled “maxRetries”](#maxretries)

Type: `UnsignedInt` · default: `3` · max: 10 · min: 1

The maximum number of times to retry failed requests. Set to 0 to disable retries

##### keyPrefix

[Section titled “keyPrefix”](#keyprefix)

Type: `String?`

A prefix that will be added to the keys of all objects stored in the blob store

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts)

Type: `Boolean` · default: `false`

Allow invalid TLS certificates when connecting to the S3 service

##### verifyAfterWrite

[Section titled “verifyAfterWrite”](#verifyafterwrite)

Type: `Boolean` · default: `true`

After each successful write, verify the object is readable on the backend. Defends against the rare case where an S3-compatible backend returns success but does not actually persist the data. Adds one extra request per write.

### @type: "Azure"

[Section titled “@type: "Azure"”](#type-azure)

Azure blob storage

##### storageAccount

[Section titled “storageAccount”](#storageaccount)

Type: `String` · required

The Azure Storage Account where blobs (e-mail messages, Sieve scripts, etc.) will be stored

##### container

[Section titled “container”](#container)

Type: `String` · required

The name of the container in the Storage Account

##### accessKey

[Section titled “accessKey”](#accesskey-1)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

The access key for the Azure Storage Account

##### sasToken

[Section titled “sasToken”](#sastoken)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

SAS Token, when not using accessKey based authentication

##### timeout

[Section titled “timeout”](#timeout-1)

Type: `Duration` · default: `"30s"`

Connection timeout to the database

##### maxRetries

[Section titled “maxRetries”](#maxretries-1)

Type: `UnsignedInt` · default: `3` · max: 10 · min: 1

The maximum number of times to retry failed requests. Set to 0 to disable retries

##### keyPrefix

[Section titled “keyPrefix”](#keyprefix-1)

Type: `String?`

A prefix that will be added to the keys of all objects stored in the blob store

### @type: "FileSystem"

[Section titled “@type: "FileSystem"”](#type-filesystem)

Filesystem

##### path

[Section titled “path”](#path)

Type: `String` · required

Where to store the data in the server’s filesystem

##### depth

[Section titled “depth”](#depth)

Type: `UnsignedInt` · default: `2` · max: 5

Maximum depth of nested directories

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

[Section titled “allowInvalidCerts”](#allowinvalidcerts-1)

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

[Section titled “allowInvalidCerts”](#allowinvalidcerts-2)

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

The BlobStore singleton is available via the `urn:stalwart:jmap` capability.

### x:BlobStore/get

[Section titled “x:BlobStore/get”](#xblobstoreget)

This is a standard [`Foo/get`](https://www.rfc-editor.org/rfc/rfc8620#section-5.1) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.1), Section 5.1.

For singletons, the `ids` argument should be the literal `singleton` (or `null` to return the single instance).

This method requires the `sysBlobStoreGet` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:BlobStore/get",          {            "ids": [              "singleton"            ]          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

### x:BlobStore/set

[Section titled “x:BlobStore/set”](#xblobstoreset)

This is a standard [`Foo/set`](https://www.rfc-editor.org/rfc/rfc8620#section-5.3) method as defined in [RFC 8620](https://www.rfc-editor.org/rfc/rfc8620#section-5.3), Section 5.3.

For singletons, only the `update` argument with id `singleton` is accepted; `create` and `destroy` arguments are rejected.

This method requires the `sysBlobStoreUpdate` [[ref/permissions|permission]].

Terminal window

```
curl -X POST https://mail.example.com/api \  -H 'Authorization: Bearer $TOKEN' \  -H 'Content-Type: application/json' \  -d '{      "methodCalls": [        [          "x:BlobStore/set",          {            "update": {              "singleton": {                "description": "updated value"              }            }          },          "c1"        ]      ],      "using": [        "urn:ietf:params:jmap:core",        "urn:stalwart:jmap"      ]    }'
```

## CLI

[Section titled “CLI”](#cli)

`stalwart-cli` wraps the same JMAP calls. See the [[management/cli|CLI reference]] for installation, authentication, and general usage.

### Fetch

[Section titled “Fetch”](#fetch)
Terminal window

```
stalwart-cli get BlobStore
```

### Update

[Section titled “Update”](#update)
Terminal window

```
stalwart-cli update BlobStore --field description='updated value'
```

## Nested types

[Section titled “Nested types”](#nested-types)

### BlobStoreBase

[Section titled “BlobStoreBase”](#blobstorebase)

Base blob store backends.

`S3`: S3-compatible. Carries the fields of [`S3Store`](#s3store).
`Azure`: Azure blob storage. Carries the fields of [`AzureStore`](#azurestore).
`FileSystem`: Filesystem. Carries the fields of [`FileSystemStore`](#filesystemstore).
`FoundationDb`: FoundationDB. Carries the fields of [`FoundationDbStore`](#foundationdbstore).
`PostgreSql`: PostgreSQL. Carries the fields of [`PostgreSqlStore`](#postgresqlstore).
`MySql`: mySQL. Carries the fields of [`MySqlStore`](#mysqlstore).

#### S3Store

[Section titled “S3Store”](#s3store)

S3-compatible blob store.

##### region

[Section titled “region”](#region-1)

Type: [`S3StoreRegion`](#s3storeregion) · required

The S3 region where the bucket resides

##### bucket

[Section titled “bucket”](#bucket-1)

Type: `String` · required

The S3 bucket where blobs (e-mail messages, Sieve scripts, etc.) will be stored

##### accessKey

[Section titled “accessKey”](#accesskey-2)

Type: `String?`

Identifies the S3 account

##### secretKey

[Section titled “secretKey”](#secretkey-1)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

The secret key for the S3 account

##### securityToken

[Section titled “securityToken”](#securitytoken-1)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Security token for temporary credentials

##### sessionToken

[Section titled “sessionToken”](#sessiontoken-1)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Temporary session token for the S3 account

##### profile

[Section titled “profile”](#profile-1)

Type: `String?`

Used when retrieving credentials from a shared credentials file. If specified, the server will use the access key ID, secret access key, and session token (if available) associated with the given profile

##### timeout

[Section titled “timeout”](#timeout-4)

Type: `Duration` · default: `"30s"`

Connection timeout to the S3 service

##### maxRetries

[Section titled “maxRetries”](#maxretries-2)

Type: `UnsignedInt` · default: `3` · max: 10 · min: 1

The maximum number of times to retry failed requests. Set to 0 to disable retries

##### keyPrefix

[Section titled “keyPrefix”](#keyprefix-2)

Type: `String?`

A prefix that will be added to the keys of all objects stored in the blob store

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts-3)

Type: `Boolean` · default: `false`

Allow invalid TLS certificates when connecting to the S3 service

##### verifyAfterWrite

[Section titled “verifyAfterWrite”](#verifyafterwrite-1)

Type: `Boolean` · default: `true`

After each successful write, verify the object is readable on the backend. Defends against the rare case where an S3-compatible backend returns success but does not actually persist the data. Adds one extra request per write.

##### S3StoreRegion

[Section titled “S3StoreRegion”](#s3storeregion)

Predefined S3 regions.

`UsEast1`: us-east-1. No additional fields.
`UsEast2`: us-east-2. No additional fields.
`UsWest1`: us-west-1. No additional fields.
`UsWest2`: us-west-2. No additional fields.
`CaCentral1`: ca-central-1. No additional fields.
`AfSouth1`: af-south-1. No additional fields.
`ApEast1`: ap-east-1. No additional fields.
`ApSouth1`: ap-south-1. No additional fields.
`ApNortheast1`: ap-northeast-1. No additional fields.
`ApNortheast2`: ap-northeast-2. No additional fields.
`ApNortheast3`: ap-northeast-3. No additional fields.
`ApSoutheast1`: ap-southeast-1. No additional fields.
`ApSoutheast2`: ap-southeast-2. No additional fields.
`CnNorth1`: cn-north-1. No additional fields.
`CnNorthwest1`: cn-northwest-1. No additional fields.
`EuNorth1`: eu-north-1. No additional fields.
`EuCentral1`: eu-central-1. No additional fields.
`EuCentral2`: eu-central-2. No additional fields.
`EuWest1`: eu-west-1. No additional fields.
`EuWest2`: eu-west-2. No additional fields.
`EuWest3`: eu-west-3. No additional fields.
`IlCentral1`: il-central-1. No additional fields.
`MeSouth1`: me-south-1. No additional fields.
`SaEast1`: sa-east-1. No additional fields.
`DoNyc3`: Digital Ocean nyc3. No additional fields.
`DoAms3`: Digital Ocean ams3. No additional fields.
`DoSgp1`: Digital Ocean sgp1. No additional fields.
`DoFra1`: Digital Ocean fra1. No additional fields.
`Yandex`: Yandex Object Storage. No additional fields.
`WaUsEast1`: Wasabi us-east-1. No additional fields.
`WaUsEast2`: Wasabi us-east-2. No additional fields.
`WaUsCentral1`: Wasabi us-central-1. No additional fields.
`WaUsWest1`: Wasabi us-west-1. No additional fields.
`WaCaCentral1`: Wasabi ca-central-1. No additional fields.
`WaEuCentral1`: Wasabi eu-central-1. No additional fields.
`WaEuCentral2`: Wasabi eu-central-2. No additional fields.
`WaEuWest1`: Wasabi eu-west-1. No additional fields.
`WaEuWest2`: Wasabi eu-west-2. No additional fields.
`WaApNortheast1`: Wasabi ap-northeast-1. No additional fields.
`WaApNortheast2`: Wasabi ap-northeast-2. No additional fields.
`WaApSoutheast1`: Wasabi ap-southeast-1. No additional fields.
`WaApSoutheast2`: Wasabi ap-southeast-2. No additional fields.
`Custom`: Custom. Carries the fields of [`S3StoreCustomRegion`](#s3storecustomregion).

##### S3StoreCustomRegion

[Section titled “S3StoreCustomRegion”](#s3storecustomregion)

Custom S3-compatible endpoint.

##### customEndpoint

[Section titled “customEndpoint”](#customendpoint)

Type: `Uri` · required

Endpoint URL

##### customRegion

[Section titled “customRegion”](#customregion)

Type: `String` · required

Region name

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

#### AzureStore

[Section titled “AzureStore”](#azurestore)

Azure Blob Storage store.

##### storageAccount

[Section titled “storageAccount”](#storageaccount-1)

Type: `String` · required

The Azure Storage Account where blobs (e-mail messages, Sieve scripts, etc.) will be stored

##### container

[Section titled “container”](#container-1)

Type: `String` · required

The name of the container in the Storage Account

##### accessKey

[Section titled “accessKey”](#accesskey-3)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

The access key for the Azure Storage Account

##### sasToken

[Section titled “sasToken”](#sastoken-1)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

SAS Token, when not using accessKey based authentication

##### timeout

[Section titled “timeout”](#timeout-5)

Type: `Duration` · default: `"30s"`

Connection timeout to the database

##### maxRetries

[Section titled “maxRetries”](#maxretries-3)

Type: `UnsignedInt` · default: `3` · max: 10 · min: 1

The maximum number of times to retry failed requests. Set to 0 to disable retries

##### keyPrefix

[Section titled “keyPrefix”](#keyprefix-3)

Type: `String?`

A prefix that will be added to the keys of all objects stored in the blob store

#### FileSystemStore

[Section titled “FileSystemStore”](#filesystemstore)

Filesystem blob store.

##### path

[Section titled “path”](#path-1)

Type: `String` · required

Where to store the data in the server’s filesystem

##### depth

[Section titled “depth”](#depth-1)

Type: `UnsignedInt` · default: `2` · max: 5

Maximum depth of nested directories

#### FoundationDbStore

[Section titled “FoundationDbStore”](#foundationdbstore)

FoundationDB data store.

##### clusterFile

[Section titled “clusterFile”](#clusterfile-1)

Type: `String?`

Path to the cluster file for the FoundationDB cluster

##### datacenterId

[Section titled “datacenterId”](#datacenterid-1)

Type: `String?`

Data center ID (optional)

##### machineId

[Section titled “machineId”](#machineid-1)

Type: `String?`

Machine ID in the FoundationDB cluster (optional)

##### transactionRetryDelay

[Section titled “transactionRetryDelay”](#transactionretrydelay-1)

Type: `Duration?`

Transaction maximum retry delay

##### transactionRetryLimit

[Section titled “transactionRetryLimit”](#transactionretrylimit-1)

Type: `UnsignedInt?` · max: 1000 · min: 1

Transaction retry limit

##### transactionTimeout

[Section titled “transactionTimeout”](#transactiontimeout-1)

Type: `Duration?`

Transaction timeout

#### PostgreSqlStore

[Section titled “PostgreSqlStore”](#postgresqlstore)

PostgreSQL data store.

##### timeout

[Section titled “timeout”](#timeout-6)

Type: `Duration?` · default: `"15s"`

Connection timeout to the database

##### useTls

[Section titled “useTls”](#usetls-2)

Type: `Boolean` · default: `false`

Use TLS to connect to the store

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts-4)

Type: `Boolean` · default: `false`

Allow invalid TLS certificates when connecting to the store

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections-2)

Type: `UnsignedInt?` · default: `10` · max: 8192 · min: 1

Maximum number of connections to the store

##### poolRecyclingMethod

[Section titled “poolRecyclingMethod”](#poolrecyclingmethod-1)

Type: [`PostgreSqlRecyclingMethod`](#postgresqlrecyclingmethod) · default: `"fast"`

Method to use when recycling connections in the pool

##### readReplicas

[Section titled “readReplicas”](#readreplicas-2)

Type: [`PostgreSqlSettings`](#postgresqlsettings)`[]` · [[server/enterprise|enterprise]]

List of read replicas for the store

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

##### PostgreSqlSettings

[Section titled “PostgreSqlSettings”](#postgresqlsettings)

PostgreSQL connection settings.

##### host

[Section titled “host”](#host-3)

Type: `String` · required

Hostname of the database server

##### port

[Section titled “port”](#port-3)

Type: `UnsignedInt` · default: `5432` · max: 65535 · min: 1

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

##### options

[Section titled “options”](#options-2)

Type: `String?`

Additional connection options

#### MySqlStore

[Section titled “MySqlStore”](#mysqlstore)

MySQL data store.

##### timeout

[Section titled “timeout”](#timeout-7)

Type: `Duration?` · default: `"15s"`

Connection timeout to the database

##### useTls

[Section titled “useTls”](#usetls-3)

Type: `Boolean` · default: `false`

Use TLS to connect to the store

##### allowInvalidCerts

[Section titled “allowInvalidCerts”](#allowinvalidcerts-5)

Type: `Boolean` · default: `false`

Allow invalid TLS certificates when connecting to the store

##### maxAllowedPacket

[Section titled “maxAllowedPacket”](#maxallowedpacket-1)

Type: `UnsignedInt?` · max: 1073741824 · min: 1024

Maximum size of a packet in bytes

##### poolMaxConnections

[Section titled “poolMaxConnections”](#poolmaxconnections-3)

Type: `UnsignedInt?` · default: `10` · max: 8192 · min: 1

Maximum number of connections to the store

##### poolMinConnections

[Section titled “poolMinConnections”](#poolminconnections-1)

Type: `UnsignedInt?` · default: `5` · max: 8192 · min: 1

Minimum number of connections to the store

##### readReplicas

[Section titled “readReplicas”](#readreplicas-3)

Type: [`MySqlSettings`](#mysqlsettings)`[]` · [[server/enterprise|enterprise]]

List of read replicas for the store

##### host

[Section titled “host”](#host-4)

Type: `String` · required

Hostname of the database server

##### port

[Section titled “port”](#port-4)

Type: `UnsignedInt` · default: `3306` · max: 65535 · min: 1

Port of the database server

##### database

[Section titled “database”](#database-4)

Type: `String` · default: `"stalwart"`

Name of the database

##### authUsername

[Section titled “authUsername”](#authusername-4)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret-4)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the store

##### MySqlSettings

[Section titled “MySqlSettings”](#mysqlsettings)

MySQL connection settings.

##### host

[Section titled “host”](#host-5)

Type: `String` · required

Hostname of the database server

##### port

[Section titled “port”](#port-5)

Type: `UnsignedInt` · default: `3306` · max: 65535 · min: 1

Port of the database server

##### database

[Section titled “database”](#database-5)

Type: `String` · default: `"stalwart"`

Name of the database

##### authUsername

[Section titled “authUsername”](#authusername-5)

Type: `String?` · default: `"stalwart"`

Username to connect to the store

##### authSecret

[Section titled “authSecret”](#authsecret-5)

Type: [`SecretKeyOptional`](#secretkeyoptional) · required

Password to connect to the store

## Enums

[Section titled “Enums”](#enums)

### PostgreSqlRecyclingMethod

[Section titled “PostgreSqlRecyclingMethod”](#postgresqlrecyclingmethod)
ValueLabel`fast`Fast recycling method`verified`Verified recycling method`clean`Clean recycling method