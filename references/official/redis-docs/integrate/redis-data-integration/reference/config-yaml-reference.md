---
title: "Redis Data Integration configuration file"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/config-yaml-reference/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/config-yaml-reference"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Data Integration configuration file"
---
# Redis Data Integration configuration file

Redis Data Integration configuration file reference

Configuration file for Redis Data Integration (RDI) source collectors and target connections.

Properties

Name
Type
Description
Required

[sources](#sources)
(Source collectors)
`object`
Source collectors that capture changes from upstream databases. Each key is a unique source identifier; the value configures one collector.

[targets](#targets)
(Target connections)
`object`
Target Redis databases where processed records are written. Each key is a target identifier; the value configures the connection.

[processors](#processors)
(Data processing configuration)
`object`, `null`
Settings that control how data is processed, including batch sizes, error handling, and performance tuning.

[secret-providers](#secret-providers)
(Secret providers)
`object`
External secret providers used to resolve `${...}` references in the configuration.

[metadata](#metadata)
(Pipeline metadata)
`object`
Optional metadata describing this pipeline, such as a display name and description.

Additional Properties: not allowed

## sources: Source collectors

Source collectors that capture changes from upstream databases. Each key is a unique source identifier; the value configures one collector.

Properties (key: `.*`)

Name
Type
Description
Required

[connection](#sourcesconnection)
(Source database connection)
`object`
Connection configuration for a non-Redis source database. The exact set of properties depends on the database type.

yes

name
(Source name)
`string`
Human-readable name for the source collector. Maximum 100 characters.
Maximal Length: `100`

no

type
(Collector type)
`string`
Type of the source collector. Use `cdc` (default) for change data capture using [Debezium](https://debezium.io/). Use `flink` for Spanner change streams using the Apache Flink-based collector. Use `riotx` for Snowflake CDC using [RIOT-X](https://redis.github.io/riotx/).
Default: `"cdc"`
Enum: `"cdc"`, `"flink"`, `"riotx"`

yes

active
(Collector enabled)
`boolean`
When `true`, the collector runs; when `false`, the collector is disabled and produces no events.
Default: `true`

no

[logging](#sourceslogging)
(Logging configuration)
`object`
Logging settings for this source collector.

no

[tables](#sourcestables)
(Tables to capture)
`object`
Tables to capture from the source database, keyed by table name. The value configures column selection and key handling for that table.

no

[schemas](#sourcesschemas)
(Schema names)
`string[]`
Schema names to capture from the source database. Maps to the underlying connector's `schema.include.list`.

no

[databases](#sourcesdatabases)
(Database names)
`string[]`
Database names to capture from the source database. Maps to the underlying connector's `database.include.list`. Applies only to MySQL, MariaDB, and MongoDB connections.

no

[advanced](#sourcesadvanced)
(Advanced configuration)
`object`
Advanced configuration that overrides the underlying engine's defaults. Only required for non-standard tuning.

no

### sources.connection: Source database connection

Connection configuration for a non-Redis source database. The exact set of properties depends on the database type.

Properties

Name
Type
Description
Required

[SQL database](#sourcesconnectionsqldatabase)
(SQL database)
`object`
Connection configuration for a supported SQL database.

[MongoDB](#sourcesconnectionmongodb)
`object`
Connection configuration for a MongoDB database.

yes

[Spanner](#sourcesconnectionspanner)
`object`
Connection configuration for a Google Cloud Spanner database.

yes

[Snowflake](#sourcesconnectionsnowflake)
`object`
Connection configuration for a Snowflake database.

yes

Example

```yaml
SQL database:
  hr:
    type: postgresql
    host: localhost
    port: 5432
    database: postgres
    user: postgres
    password: postgres
MongoDB:
  mongodb-source:
    type: mongodb
    connection_string: mongodb://localhost:27017/?replicaSet=rs0
    user: debezium
    password: dbz
    database: db1,db2
Spanner:
  spanner-source:
    type: spanner
    project_id: example-12345
    instance_id: example
    database_id: example
    change_streams:
      change_stream_all:
        retention_period_hours: 24
Snowflake:
  snowflake:
    type: snowflake
    url: jdbc:snowflake://myaccount.snowflakecomputing.com/
    user: myuser
    password: mypassword
    database: MYDB
    warehouse: COMPUTE_WH
```

#### sources.connection.SQL database: SQL database

Connection configuration for a supported SQL database.

Properties

Name
Type
Description
Required

type
(Database type)
`string`
SQL database engine.
Enum: `"mariadb"`, `"mysql"`, `"oracle"`, `"postgresql"`, `"sqlserver"`

host
(Database host)
`string`
Hostname or IP address of the SQL database server.

port
(Database port)
`integer`
Network port on which the SQL database server is listening.
Minimum: `1`
Maximum: `65535`

database
(Database name)
`string`
Name of the database to connect to.

user
(Database user)
`string`
Username for authentication to the SQL database.

password
(Database password)
`string`
Password for authentication to the SQL database.

Additional Properties: not allowed
Example

```yaml
hr:
  type: postgresql
  host: localhost
  port: 5432
  database: postgres
  user: postgres
  password: postgres
```

Example

```yaml
my-oracle:
  type: oracle
  host: 172.17.0.4
  port: 1521
  user: c##dbzuser
  password: dbz
```

#### sources.connection.MongoDB: MongoDB

Connection configuration for a MongoDB database.

Properties

Name
Type
Description
Required

type
(Database type)
`string`
Database type identifier. Always `mongodb` for this connection.
Constant Value: `"mongodb"`

yes

connection_string
`string`
MongoDB connection URI including host, port, and any connection options.

yes

user
(MongoDB user)
`string`
Username for authentication to MongoDB.

no

password
(MongoDB password)
`string`
Password for authentication to MongoDB.

no

database
(MongoDB databases)
`string`
Comma-separated list of MongoDB databases to monitor.

no

Additional Properties: not allowed
Example

```yaml
mongodb-source:
  type: mongodb
  connection_string: mongodb://localhost:27017/?replicaSet=rs0
  user: debezium
  password: dbz
  database: db1,db2
```

#### sources.connection.Spanner: Spanner

Connection configuration for a Google Cloud Spanner database.

Properties

Name
Type
Description
Required

type
(Database type)
`string`
Database type identifier. Always `spanner` for this connection.
Constant Value: `"spanner"`

yes

project_id
(Spanner project ID)
`string`
Google Cloud project ID that hosts the Spanner instance.

yes

instance_id
(Spanner instance ID)
`string`
Spanner instance identifier within the project.

yes

database_id
(Spanner database ID)
`string`
Spanner database identifier within the instance.

yes

emulator_host
(Spanner emulator host)
`string`
Host and port of the Spanner emulator. Used for local development; leave unset against real Spanner.

no

use_credentials_file
`boolean`
When `true`, RDI authenticates using a service account credentials file; when `false`, it uses application default credentials.
Default: `false`

no

[change_streams](#sourcesconnectionspannerchange_streams)
(Change streams configuration)
`object`
Spanner change streams to capture, keyed by change stream name.

yes

Additional Properties: not allowed
Example

```yaml
spanner-source:
  type: spanner
  project_id: example-12345
  instance_id: example
  database_id: example
  change_streams:
    change_stream_all:
      retention_period_hours: 24
```

##### sources.connection.Spanner.change_streams: Change streams configuration

Spanner change streams to capture, keyed by change stream name.

Additional Properties

Name
Type
Description
Required

[Additional Properties](#sourcesconnectionspannerchange_streamsadditionalproperties)
`object`, `null`

Minimal Properties: 1

###### sources.connection.Spanner.change_streams.additionalProperties: object,null

Properties

Name
Type
Description
Required

retention_period_hours
(Change stream retention period hours)
`integer`, `string`
Retention period for the change stream, in hours.
Pattern: `^\${.*}$`
Minimum: `1`

Additional Properties: not allowed

#### sources.connection.Snowflake: Snowflake

Connection configuration for a Snowflake database.

Properties

Name
Type
Description
Required

type
(Database type)
`string`
Database type identifier. Always `snowflake` for this connection.
Constant Value: `"snowflake"`

yes

url
(JDBC URL)
`string`
Snowflake JDBC connection URL, for example `jdbc:snowflake://account.snowflakecomputing.com/`.

yes

user
(Snowflake user)
`string`
Username for authentication to Snowflake.

yes

password
(Snowflake password)
`string`
Password for authentication to Snowflake. For key-pair authentication, omit this field and provide the private key via the `source-db-ssl` secret (`client.key` field).

no

database
(Snowflake database)
`string`
Name of the Snowflake database to connect to.

yes

warehouse
(Snowflake warehouse)
`string`
Name of the Snowflake warehouse used for compute.

yes

role
(Snowflake role)
`string`
Snowflake role used for the connection.

no

cdcDatabase
(CDC database)
`string`
Database hosting the CDC streams. Defaults to the main `database` if not set.

no

cdcSchema
(CDC schema)
`string`
Schema hosting the CDC streams. Defaults to the main schema if not set.

no

Additional Properties: not allowed
Example

```yaml
snowflake:
  type: snowflake
  url: jdbc:snowflake://myaccount.snowflakecomputing.com/
  user: myuser
  password: mypassword
  database: MYDB
  warehouse: COMPUTE_WH
```

### sources.logging: Logging configuration

Logging settings for this source collector.

Properties

Name
Type
Description
Required

level
(Logging level)
`string`
Log verbosity for the source collector.
Default: `"info"`
Enum: `"trace"`, `"debug"`, `"info"`, `"warn"`, `"error"`

Additional Properties: not allowed
Example

```yaml
level: info
```

### sources.tables: Tables to capture

Tables to capture from the source database, keyed by table name. The value configures column selection and key handling for that table.

Additional Properties

Name
Type
Description
Required

[Additional Properties](#sourcestablesadditionalproperties)
`object`, `null`

Minimal Properties: 1

#### sources.tables.additionalProperties: object,null

Properties

Name
Type
Description
Required

snapshot_sql
`string`
Custom SQL statement used during the initial snapshot, giving fine-grained control over the data captured.

[columns](#sourcestablesadditionalpropertiescolumns)
(Columns to capture)
`string[]`
List of specific columns to capture for changes. If not specified, all columns will be captured. For RIOTX Snowflake sources, this is rendered as per-table `--table-columns` projection. Note: This property cannot be used for MongoDB connections

[exclude_columns](#sourcestablesadditionalpropertiesexclude_columns)
(Columns to exclude)
`string[]`
Specific columns to exclude from capture. When omitted, no columns are excluded. Only supported for MongoDB connections.

[keys](#sourcestablesadditionalpropertieskeys)
(Message keys)
`string[]`
Optional list of columns to use as a composite unique identifier. For RIOTX Snowflake sources, this is rendered as per-table `--table-keys`. Only required when the table lacks a primary key or unique constraint. Must form a unique combination of fields

Additional Properties: not allowed

##### sources.tables.additionalProperties.columns[]: Columns to capture

List of specific columns to capture for changes. If not specified, all columns will be captured. For RIOTX Snowflake sources, this is rendered as per-table `--table-columns` projection. Note: This property cannot be used for MongoDB connections

##### sources.tables.additionalProperties.exclude_columns[]: Columns to exclude

Specific columns to exclude from capture. When omitted, no columns are excluded. Only supported for MongoDB connections.

##### sources.tables.additionalProperties.keys[]: Message keys

Optional list of columns to use as a composite unique identifier. For RIOTX Snowflake sources, this is rendered as per-table `--table-keys`. Only required when the table lacks a primary key or unique constraint. Must form a unique combination of fields

### sources.schemas[]: Schema names

Schema names to capture from the source database. Maps to the underlying connector's `schema.include.list`.

### sources.databases[]: Database names

Database names to capture from the source database. Maps to the underlying connector's `database.include.list`. Applies only to MySQL, MariaDB, and MongoDB connections.

### sources.advanced: Advanced configuration

Advanced configuration that overrides the underlying engine's defaults. Only required for non-standard tuning.

Properties

Name
Type
Description
Required

[sink](#sourcesadvancedsink)
(RDI Collector stream writer configuration)
`object`
Advanced configuration properties for the RDI Collector stream writer connection and behaviour. Applies to the `cdc` and `flink` collector types.

[source](#sourcesadvancedsource)
(Advanced source settings)
`object`
Advanced configuration properties for the source database connection and CDC behavior. Applies to the `cdc` and `flink` collector types.

[quarkus](#sourcesadvancedquarkus)
(Quarkus runtime settings)
`object`
Advanced configuration properties for the Quarkus runtime that hosts Debezium Server. Only applies to the `cdc` collector type. See the [Debezium Server documentation](https://debezium.io/documentation/reference/stable/operations/debezium-server.html) for runtime configuration options. When using a property from that page, omit the `quarkus.` prefix.

[flink](#sourcesadvancedflink)
(Advanced Flink settings)
`object`
Advanced configuration properties forwarded to the Flink runtime that hosts the collector. Any property listed in the [Flink configuration documentation](https://nightlies.apache.org/flink/flink-docs-release-2.0/docs/deployment/config/) can be set here and will override the RDI default. Only applies to the `flink` collector type.

[resources](#sourcesadvancedresources)
(Collector resource settings)
`object`
Compute resources allocated to the collector. Only applies to the `cdc` collector type.

[riotx](#sourcesadvancedriotx)
(Advanced RIOT-X settings)
`object`
Advanced configuration properties for the RIOT-X Snowflake collector. Only applies to the `riotx` collector type.

java_options
(Advanced Java options)
`string`
These Java options will be passed to the command line command when launching the source collector. Only applies to the `cdc` collector type.

Additional Properties: not allowed
Minimal Properties: 1
Example

```yaml
sink:
  redis.batch.size: 1000
  redis.flush.interval.ms: 100
  redis.connection.timeout.ms: 2000
  redis.socket.timeout.ms: 2000
  redis.retry.max.attempts: 5
  redis.retry.initial.delay.ms: 100
  redis.retry.max.delay.ms: 3000
  redis.retry.backoff.multiplier: 2
  redis.oom.retry.initial.delay.ms: 1000
  redis.oom.retry.max.delay.ms: 10000
  redis.oom.retry.backoff.multiplier: 2
  redis.wait.enabled: false
  redis.wait.write.timeout.ms: 1000
  redis.wait.retry.enabled: false
  redis.wait.retry.delay.ms: 1000
source:
  snapshot.max.threads: 1
  poll.interval.ms: 500
  snapshot.fetch.size: 10000
  max.batch.size: 2048
  max.queue.size: 8192
  heartbeat.interval.ms: 0
  lob.enabled: false
  publication.autocreate.mode: all_tables
  publication.name: dbz_publication
  slot.name: debezium
  spanner.version.retention.period.hours: 1
  spanner.fetch.timeout.ms: 500
  spanner.fetch.heartbeat.ms: 100
  spanner.max.rows.per.partition: 10000
  spanner.dialect: GOOGLESQL
quarkus: {}
flink:
  taskmanager.numberOfTaskSlots: 1
resources: {}
riotx:
  poll: 30s
  snapshot: INITIAL
  streamPrefix: 'data:'
  clearOffset: false
  count: 0
```

#### sources.advanced.sink: RDI Collector stream writer configuration

Advanced configuration properties for the RDI Collector stream writer connection and behaviour. Applies to the `cdc` and `flink` collector types.For the `cdc` collector type, see the full list of properties at [Debezium Server â Redis Stream sink](https://debezium.io/documentation/reference/stable/operations/debezium-server.html#_redis_stream). When using a property from that page, omit the `debezium.sink.` prefix.The properties listed below only apply to the `flink` collector type.

Properties

Name
Type
Description
Required

redis.batch.size
(Sink batch size)
`integer`
Maximum number of records the collector sink writes to Redis in a single batch.
Default: `1000`
Minimum: `1`

redis.flush.interval.ms
(Sink flush interval)
`integer`
Maximum time in milliseconds the collector sink waits to fill a batch before flushing it to Redis.
Default: `100`
Minimum: `1`

redis.connection.timeout.ms
(Sink connection timeout)
`integer`
Connection timeout in milliseconds for the target Redis client used by the collector sink.
Default: `2000`
Minimum: `1`

redis.socket.timeout.ms
(Sink socket timeout)
`integer`
Socket read/write timeout in milliseconds for the target Redis client used by the collector sink.
Default: `2000`
Minimum: `1`

redis.retry.max.attempts
(Sink retry max attempts)
`integer`
Maximum number of retry attempts for failed Redis operations.
Default: `5`
Minimum: `1`

redis.retry.initial.delay.ms
(Sink retry initial delay)
`integer`
Initial delay in milliseconds before the first retry of a failed Redis operation.
Default: `100`
Minimum: `1`

redis.retry.max.delay.ms
(Sink retry max delay)
`integer`
Maximum delay in milliseconds between retry attempts for failed Redis operations.
Default: `3000`
Minimum: `1`

redis.retry.backoff.multiplier
(Sink retry backoff multiplier)
`number`
Exponential backoff multiplier between retry attempts for failed Redis operations.
Default: `2`
Minimum: `1`

redis.oom.retry.initial.delay.ms
(Sink OOM retry initial delay)
`integer`
Initial delay in milliseconds before the first retry after a Redis out-of-memory error.
Default: `1000`
Minimum: `1`

redis.oom.retry.max.delay.ms
(Sink OOM retry max delay)
`integer`
Maximum delay in milliseconds between retry attempts after a Redis out-of-memory error.
Default: `10000`
Minimum: `1`

redis.oom.retry.backoff.multiplier
(Sink OOM retry backoff multiplier)
`number`
Exponential backoff multiplier between retry attempts after a Redis out-of-memory error.
Default: `2`
Minimum: `1`

redis.wait.enabled
(Sink replica wait enabled)
`boolean`
When `true`, the collector verifies that each write has been replicated to the configured number of Redis replica shards before acknowledging it.
Default: `false`

redis.wait.write.timeout.ms
(Sink replica wait timeout)
`integer`
Maximum time in milliseconds to wait for replica write acknowledgements.
Default: `1000`
Minimum: `1`

redis.wait.retry.enabled
(Sink replica wait retry enabled)
`boolean`
When `true`, the collector keeps retrying a write until replica acknowledgement succeeds; when `false`, it gives up after the first failure.
Default: `false`

redis.wait.retry.delay.ms
(Sink replica wait retry delay)
`integer`
Delay in milliseconds between replica wait retry attempts.
Default: `1000`
Minimum: `1`

Additional Properties

Name
Type
Description
Required

Additional Properties
`string`, `number`, `boolean`

Minimal Properties: 1
Example

```yaml
redis.batch.size: 1000
redis.flush.interval.ms: 100
redis.connection.timeout.ms: 2000
redis.socket.timeout.ms: 2000
redis.retry.max.attempts: 5
redis.retry.initial.delay.ms: 100
redis.retry.max.delay.ms: 3000
redis.retry.backoff.multiplier: 2
redis.oom.retry.initial.delay.ms: 1000
redis.oom.retry.max.delay.ms: 10000
redis.oom.retry.backoff.multiplier: 2
redis.wait.enabled: false
redis.wait.write.timeout.ms: 1000
redis.wait.retry.enabled: false
redis.wait.retry.delay.ms: 1000
```

#### sources.advanced.source: Advanced source settings

Advanced configuration properties for the source database connection and CDC behavior. Applies to the `cdc` and `flink` collector types.For the `cdc` collector type, available properties depend on the source database â refer to the relevant Debezium connector documentation: [MySQL](https://debezium.io/documentation/reference/stable/connectors/mysql.html), [MariaDB](https://debezium.io/documentation/reference/stable/connectors/mariadb.html), [PostgreSQL](https://debezium.io/documentation/reference/stable/connectors/postgresql.html), [Oracle](https://debezium.io/documentation/reference/stable/connectors/oracle.html), [SQL Server](https://debezium.io/documentation/reference/stable/connectors/sqlserver.html), [Db2](https://debezium.io/documentation/reference/stable/connectors/db2.html), [MongoDB](https://debezium.io/documentation/reference/stable/connectors/mongodb.html). When using a property from those pages, omit the `debezium.source.` prefix.The named properties below cover the most commonly tuned settings: `spanner.*` properties apply to the `flink` collector type, all others apply to the `cdc` collector type. Any other property from the Debezium documentation can still be set as a free-form key-value pair.

Properties

Name
Type
Description
Required

record.processing.threads
(Record processing threads)
`integer`
Controls how many worker threads process captured records before they are written downstream.
Minimum: `1`

snapshot.max.threads
(Snapshot max threads)
`integer`
Sets the maximum number of threads used while taking the initial snapshot.
Default: `1`
Minimum: `1`

poll.interval.ms
(Poll interval ms)
`integer`
Defines how often the collector polls the source for new changes.
Default: `500`
Minimum: `1`

snapshot.fetch.size
(Snapshot fetch size)
`integer`
Defines how many rows are fetched per batch during the initial snapshot.
Default: `10000`
Minimum: `1`

max.batch.size
(Max batch size)
`integer`
Caps how many records are processed together in a single batch.
Default: `2048`
Minimum: `1`

max.queue.size
(Max queue size)
`integer`
Limits how many records can be buffered in memory before processing catches up.
Default: `8192`
Minimum: `1`

heartbeat.interval.ms
(Heartbeat interval ms)
`integer`
Sets how often heartbeat events are emitted to keep change tracking active. Use 0 to disable them.
Default: `0`
Minimum: `0`

heartbeat.action.query
(Heartbeat action query)
`string`
SQL query executed on the source whenever a heartbeat is emitted.

lob.enabled
(Lob enabled)
`boolean`
Determines whether large object columns are included in change capture.
Default: `false`

gtid.source.includes
(GTID source includes)
`string`
Restricts MySQL GTID processing to the listed source UUIDs.

publication.autocreate.mode
(Publication autocreate mode)
`string`
Controls whether and how the PostgreSQL publication is created or updated automatically.
Default: `"all_tables"`
Enum: `"all_tables"`, `"filtered"`, `"disabled"`

publication.name
(Publication name)
`string`
Sets the PostgreSQL logical replication publication name used by the collector.
Default: `"dbz_publication"`

slot.name
(Slot name)
`string`
Sets the PostgreSQL replication slot name the collector reads from.
Default: `"debezium"`

spanner.version.retention.period.hours
(Spanner version retention period)
`integer`
Retention period in hours for Spanner change stream versions. Determines how far back the collector can resume after an outage.
Default: `1`
Minimum: `1`

spanner.fetch.timeout.ms
(Spanner fetch timeout)
`integer`
Timeout in milliseconds for a single change stream fetch request to Spanner.
Default: `500`
Minimum: `1`

spanner.fetch.heartbeat.ms
(Spanner fetch heartbeat interval)
`integer`
Interval in milliseconds at which Spanner sends heartbeat records when no data changes are available.
Default: `100`
Minimum: `1`

spanner.max.rows.per.partition
(Spanner max rows per partition)
`integer`
Maximum number of rows the collector reads from a single Spanner change stream partition before yielding.
Default: `10000`
Minimum: `1`

spanner.dialect
(Spanner SQL dialect)
`string`
SQL dialect of the Spanner database. Use `GOOGLESQL` for Google Standard SQL or `POSTGRESQL` for the PostgreSQL interface.
Default: `"GOOGLESQL"`
Enum: `"GOOGLESQL"`, `"POSTGRESQL"`

Additional Properties

Name
Type
Description
Required

Additional Properties
`string`, `number`, `boolean`

Minimal Properties: 1
Example

```yaml
snapshot.max.threads: 1
poll.interval.ms: 500
snapshot.fetch.size: 10000
max.batch.size: 2048
max.queue.size: 8192
heartbeat.interval.ms: 0
lob.enabled: false
publication.autocreate.mode: all_tables
publication.name: dbz_publication
slot.name: debezium
spanner.version.retention.period.hours: 1
spanner.fetch.timeout.ms: 500
spanner.fetch.heartbeat.ms: 100
spanner.max.rows.per.partition: 10000
spanner.dialect: GOOGLESQL
```

#### sources.advanced.quarkus: Quarkus runtime settings

Advanced configuration properties for the Quarkus runtime that hosts Debezium Server. Only applies to the `cdc` collector type. See the [Debezium Server documentation](https://debezium.io/documentation/reference/stable/operations/debezium-server.html) for runtime configuration options. When using a property from that page, omit the `quarkus.` prefix.

Additional Properties

Name
Type
Description
Required

Additional Properties
`string`, `number`, `boolean`

Minimal Properties: 1

#### sources.advanced.flink: Advanced Flink settings

Advanced configuration properties forwarded to the Flink runtime that hosts the collector. Any property listed in the [Flink configuration documentation](https://nightlies.apache.org/flink/flink-docs-release-2.0/docs/deployment/config/) can be set here and will override the RDI default. Only applies to the `flink` collector type.The properties listed below are the ones most likely to require adjustment. Changing any other Flink property is not recommended unless instructed by Redis support.

Properties

Name
Type
Description
Required

parallelism.default
(Default parallelism)
`integer`
Default parallelism for Flink jobs and operators. When unset, Flink uses the number of available task slots across all task managers (`taskManager.replicas Ã taskmanager.numberOfTaskSlots`). See [parallel execution](https://nightlies.apache.org/flink/flink-docs-release-2.0/docs/dev/datastream/execution/parallel/).
Minimum: `1`

taskmanager.numberOfTaskSlots
(Task slots per task manager)
`integer`
Number of parallel task slots per task manager pod. Each slot can run one parallel pipeline instance, so this caps the parallelism a single task manager can absorb. See [task slots and resources](https://nightlies.apache.org/flink/flink-docs-release-2.0/docs/concepts/flink-architecture/#task-slots-and-resources).
Default: `1`
Minimum: `1`

taskmanager.memory.process.size
(Task manager process memory)
`string`
Total memory budget for each task manager JVM process, expressed with a unit suffix such as `2048m` or `4g`. See [task manager memory configuration](https://nightlies.apache.org/flink/flink-docs-release-2.0/docs/deployment/memory/mem_setup_tm/).

Additional Properties

Name
Type
Description
Required

Additional Properties
`string`, `number`, `boolean`

Minimal Properties: 1
Example

```yaml
taskmanager.numberOfTaskSlots: 1
```

#### sources.advanced.resources: Collector resource settings

Compute resources allocated to the collector. Only applies to the `cdc` collector type.

Properties

Name
Type
Description
Required

cpu
(CPU resource value)
`string`
CPU request for the collector container, for example `1` or `500m`.

memory
(Memory resource value)
`string`
Memory request for the collector container, for example `1024Mi` or `2Gi`.

Additional Properties: not allowed
Minimal Properties: 1

#### sources.advanced.riotx: Advanced RIOT-X settings

Advanced configuration properties for the RIOT-X Snowflake collector. Only applies to the `riotx` collector type.

Properties

Name
Type
Description
Required

poll
(Polling interval)
`string`
Interval between polls for new stream changes, for example `30s` or `PT30S`.
Default: `"30s"`

snapshot
(Snapshot mode)
`string`
Initial-load behavior. `INITIAL` performs a one-time snapshot before streaming; `NEVER` skips the snapshot.
Default: `"INITIAL"`
Enum: `"INITIAL"`, `"NEVER"`

streamPrefix
(Redis stream key prefix)
`string`
Prefix used when constructing Redis stream keys, for example `data:`.
Default: `"data:"`

streamLimit
(Maximum stream length)
`integer`
Maximum number of entries kept in each Redis stream before older entries are trimmed.
Minimum: `1`

[keyColumns](#sourcesadvancedriotxkeycolumns)
(Key columns)
`string[]`
Deprecated RIOTX global fallback list of columns to use as message keys for every captured table. Prefer `tables.<schema.table>.keys`

clearOffset
(Clear existing offset)
`boolean`
When `true`, the stored offset is cleared on startup, forcing a fresh read.
Default: `false`

count
(Record count limit)
`integer`
Maximum number of records to process. Set to `0` for unlimited.
Default: `0`
Minimum: `0`

Additional Properties: not allowed
Minimal Properties: 1
Example

```yaml
poll: 30s
snapshot: INITIAL
streamPrefix: 'data:'
clearOffset: false
count: 0
```

##### sources.advanced.riotx.keyColumns[]: Key columns

Deprecated RIOTX global fallback list of columns to use as message keys for every captured table. Prefer `tables.<schema.table>.keys`

## targets: Target connections

Target Redis databases where processed records are written. Each key is a target identifier; the value configures the connection.

Properties (key: `.*`)

Name
Type
Description
Required

[connection](#targetsconnection)
(Database connection)
`object`
Connection configuration for a Redis database.

yes

name
(Target name)
`string`
Human-readable name for the target connection. Maximum 100 characters.
Maximal Length: `100`

no

### targets.connection: Database connection

Connection configuration for a Redis database.

Properties

Name
Type
Description
Required

type
(Database type)

Database type identifier. Always `redis` for this connection.
Constant Value: `"redis"`

yes

host
(Database host)
`string`
Hostname or IP address of the Redis server.

yes

port
(Database port)
`integer`
Network port on which the Redis server is listening.
Minimum: `1`
Maximum: `65535`

yes

user
(Database user)
`string`
Username for authentication to the Redis database.

no

password
(Database password)
`string`
Password for authentication to the Redis database.

no

key
(Private key file)
`string`
Path to the private key file used for SSL/TLS client authentication.

no

key_password
(Private key password)
`string`
Password used to decrypt the private key file.

no

cert
(Client certificate)
`string`
Path to the client certificate file used for SSL/TLS client authentication.

no

cacert
(CA certificate)
`string`
Path to the Certificate Authority (CA) certificate file used to verify the server's TLS certificate.

no

Additional Properties: not allowed
Minimal Properties: 3
If property key is defined, property/ies cert is/are required.
If property cert is defined, property/ies key is/are required.
If property key_password is defined, property/ies key is/are required.

## processors: Data processing configuration

Settings that control how data is processed, including batch sizes, error handling, and performance tuning.

Properties

Name
Type
Description
Required

type
(Processor type)
`string`
Processor implementation to run. `classic` runs the classic processor; `flink` runs the Apache Flink-based processor.
Default: `"classic"`
Enum: `"classic"`, `"flink"`

read_batch_size
`integer`, `string`
Maximum number of records read from the source streams in a single batch.
Default: `2000`
Pattern: `^\${.*}$`
Minimum: `1`

read_batch_timeout_ms
(Read batch timeout)
`integer`
Maximum time in milliseconds to wait for a batch to fill before processing it.
Default: `100`
Minimum: `1`

duration
(Batch duration limit)
`integer`, `string`
(DEPRECATED)
This property has no effect; use `read_batch_timeout_ms` instead.
Default: `100`
Pattern: `^\${.*}$`
Minimum: `1`

write_batch_size
`integer`, `string`
Maximum number of records written to the target Redis database in a single batch.
Default: `200`
Pattern: `^\${.*}$`
Minimum: `1`

enable_async_processing
`boolean`
When `true`, the processor handles batches asynchronously to improve throughput. Classic processor only.
Default: `true`

batch_queue_size
`integer`
Maximum number of batches queued for processing. Classic processor only.
Default: `3`
Minimum: `1`

ack_queue_size
`integer`
Maximum number of batches queued for asynchronous acknowledgement. Classic processor only.
Default: `10`
Minimum: `1`

dedup
(Enable deduplication)
`boolean`
When `true`, the processor deduplicates incoming records. Classic processor only.
Default: `false`

dedup_max_size
(Deduplication set size)
`integer`
Maximum number of entries kept in the deduplication set. Classic processor only.
Default: `1024`
Minimum: `1`

dedup_strategy
(Deduplication strategy)
`string`
(DEPRECATED)
This property has no effect â the only supported strategy is `ignore`. Remove it from the configuration. Classic processor only.
Default: `"ignore"`
Enum: `"reject"`, `"ignore"`

error_handling
(Error handling strategy)
`string`
Strategy for handling failed records. `ignore` silently drops them; `dlq` writes them to the dead-letter queue.
Default: `"dlq"`

dlq_max_messages
(DLQ message limit)
`integer`, `string`
Maximum number of messages stored per dead-letter queue stream.
Default: `1000`
Pattern: `^\${.*}$`
Minimum: `1`

target_data_type
(Target Redis data type)
`string`
Data type used to store target records in Redis. `hash` writes a Redis Hash; `json` writes a RedisJSON document and requires the RedisJSON module.
Default: `"hash"`

json_update_strategy
`string`
Strategy for updating existing JSON documents in Redis. `replace` overwrites the entire document; `merge` merges incoming fields into it.
Default: `"replace"`

use_native_json_merge
(Use native JSON merge from RedisJSON module)
`boolean`
Controls whether JSON merge operations use the native `JSON.MERGE` command (when `true`) or Lua scripts (when `false`). Introduced in RDI 1.15.0. The native command provides 2x performance improvement but handles null values differently:

Previous behavior (Lua merge): When merging `{"field1": "value1", "field2": "value2"}` with `{"field2": null, "field3": "value3"}`, the result was `{"field1": "value1", "field2": null, "field3": "value3"}` (null value is preserved).

New behavior (JSON.MERGE): The same merge produces `{"field1": "value1", "field3": "value3"}` (null value removes the field, following [RFC 7396](https://datatracker.ietf.org/doc/html/rfc7396)).

Note: The native `JSON.MERGE` command requires RedisJSON 2.6.0 or higher. If the target database has an older version of RedisJSON, RDI automatically falls back to Lua-based merge operations regardless of this setting.

Impact: If your application logic distinguishes between a field with a `null` value and a missing field, you may need to adjust your data handling. This follows the JSON Merge Patch RFC standard but differs from the previous Lua implementation. Set to `false` to revert to the previous Lua-based merge behavior if needed.

The Flink processor always uses the native `JSON.MERGE` command when the target database supports it. Classic processor only.
Default: `true`

initial_sync_processes
`integer`, `string`
Number of parallel processes used to perform the initial data synchronization. For the Flink processor, parallelism is controlled by Flink properties instead. Classic processor only.
Default: `4`
Pattern: `^\${.*}$`
Minimum: `1`
Maximum: `32`

idle_sleep_time_ms
(Idle sleep interval)
`integer`, `string`
Time in milliseconds to sleep between processing batches when idle. Classic processor only.
Default: `200`
Pattern: `^\${.*}$`
Minimum: `1`
Maximum: `999999`

idle_streams_check_interval_ms
(Idle streams check interval)
`integer`, `string`
Time in milliseconds between checks for new streams when the processor is idle. For the Flink processor, use `processors.advanced.source.discovery.interval.ms` instead to configure a single discovery interval regardless of load. Classic processor only.
Default: `1000`
Pattern: `^\${.*}$`
Minimum: `1`
Maximum: `999999`

busy_streams_check_interval_ms
(Busy streams check interval)
`integer`, `string`
Time in milliseconds between checks for new streams when the processor is busy. For the Flink processor, use `processors.advanced.source.discovery.interval.ms` instead to configure a single discovery interval regardless of load. Classic processor only.
Default: `5000`
Pattern: `^\${.*}$`
Minimum: `1`
Maximum: `999999`

retry_max_attempts
(Maximum retry attempts)
`integer`, `string`
Maximum number of attempts for a failed write to the target Redis database before giving up.
Default: `5`
Pattern: `^\${.*}$`
Minimum: `1`

retry_initial_delay_ms
(Initial retry delay)
`integer`, `string`
Initial delay in milliseconds before the first retry of a failed write.
Default: `1000`
Pattern: `^\${.*}$`
Minimum: `1`
Maximum: `999999`

retry_max_delay_ms
(Maximum retry delay)
`integer`, `string`
Maximum delay in milliseconds between retry attempts.
Default: `10000`
Pattern: `^\${.*}$`
Minimum: `1`
Maximum: `999999`

wait_enabled
(Enable replica wait)
`boolean`
When `true`, RDI verifies that each write has been replicated to the target database's replica shards before acknowledging it. Enable this only when target database replication is enabled and a healthy replica is available. For the Flink processor, `processors.advanced.target.wait.enabled` takes priority.
Default: `false`

wait_timeout
(Replica wait timeout)
`integer`, `string`
Maximum time in milliseconds to wait for replica write verification on the target database.
Default: `1000`
Pattern: `^\${.*}$`
Minimum: `1`

retry_on_replica_failure
`boolean`
When `true`, RDI keeps retrying a write until replica replication is confirmed; when `false`, it gives up after the first failure.
Default: `true`

on_failed_retry_interval
(Retry interval on failure)
`integer`, `string`
(DEPRECATED)
This property has no effect; remove it from the configuration.
Default: `5`
Pattern: `^\${.*}$`
Minimum: `1`

[logging](#processorslogging)
(Logging configuration)
`object`
Logging settings for the processor. Flink processor only.

[advanced](#processorsadvanced)
(Advanced configuration)
`object`
Advanced configuration for fine-tuning the processor. All properties under `advanced` apply to the Flink processor only and are silently ignored by the classic processor.

Additional Properties: not allowed

### processors.logging: Logging configuration

Logging settings for the processor. Flink processor only.

Properties

Name
Type
Description
Required

level
(Logging level)
`string`
Log verbosity for the processor.
Default: `"info"`
Enum: `"trace"`, `"debug"`, `"info"`, `"warn"`, `"error"`

Additional Properties: not allowed
Example

```yaml
level: info
```

### processors.advanced: Advanced configuration

Advanced configuration for fine-tuning the processor. All properties under `advanced` apply to the Flink processor only and are silently ignored by the classic processor.

Properties

Name
Type
Description
Required

[source](#processorsadvancedsource)
(Advanced source settings)
`object`
Advanced configuration properties for the source Redis client and streams reader. Flink processor only.

[target](#processorsadvancedtarget)
(Advanced target settings)
`object`
Advanced configuration properties for the target Redis client and sink. Flink processor only.

[dlq](#processorsadvanceddlq)
(Advanced DLQ settings)
`object`
Advanced configuration properties for the DLQ Redis client and sink. Flink processor only.

[processor](#processorsadvancedprocessor)
(Advanced processor settings)
`object`
Advanced configuration properties for the processor. Flink processor only.

[flink](#processorsadvancedflink)
(Advanced Flink settings)
`object`
Advanced configuration properties forwarded to the underlying Flink runtime. Any property listed in the [Flink configuration documentation](https://nightlies.apache.org/flink/flink-docs-release-2.0/docs/deployment/config/) can be set here and will override the RDI default. Flink processor only.

[resources](#processorsadvancedresources)
(Advanced resource settings)
`object`
Compute resources allocated to the Flink job, such as the number of task manager pods. Flink processor only.

Additional Properties: not allowed
Minimal Properties: 1
Example

```yaml
source:
  stream.name.pattern: data:*
  discovery.interval.ms: 1000
  batch.size: 2000
  batch.timeout.ms: 100
  connection.timeout.ms: 2000
  socket.timeout.ms: 2000
  retry.max.attempts: 5
  retry.initial.delay.ms: 100
  retry.max.delay.ms: 3000
  retry.backoff.multiplier: 2
target:
  batch.size: 200
  flush.interval.ms: 100
  connection.timeout.ms: 2000
  socket.timeout.ms: 2000
  retry.max.attempts: 5
  retry.initial.delay.ms: 1000
  retry.max.delay.ms: 10000
  retry.backoff.multiplier: 2
  wait.enabled: false
  wait.write.timeout.ms: 1000
  wait.retry.enabled: true
  wait.retry.delay.ms: 1000
dlq:
  max.len: 1000
  batch.size: 100
  flush.interval.ms: 100
  connection.timeout.ms: 2000
  socket.timeout.ms: 2000
  retry.max.attempts: 1
  retry.initial.delay.ms: 100
  retry.max.delay.ms: 3000
  retry.backoff.multiplier: 2
  wait.enabled: false
  wait.write.timeout.ms: 1000
  wait.retry.enabled: false
  wait.retry.delay.ms: 1000
processor:
  default.data.type: hash
  default.json.update.strategy: replace
  dlq.enabled: true
flink:
  taskmanager.numberOfTaskSlots: 1
  taskmanager.memory.process.size: 2048m
resources:
  taskManager: {}
```

#### processors.advanced.source: Advanced source settings

Advanced configuration properties for the source Redis client and streams reader. Flink processor only.

Properties

Name
Type
Description
Required

stream.name.pattern
(Source stream name pattern)
`string`
Glob pattern used to discover input streams in the source Redis database, for example `data:*`.
Default: `"data:*"`

discovery.interval.ms
(Stream discovery interval)
`integer`
Time in milliseconds between checks for new input streams. Replaces the classic `processors.idle_streams_check_interval_ms` and `processors.busy_streams_check_interval_ms` properties.
Default: `1000`
Minimum: `0`

batch.size
(Source batch size)
`integer`
Maximum number of records the source operator reads in a single batch. Alias for `processors.read_batch_size`; takes priority when both are set.
Default: `2000`
Minimum: `1`

batch.timeout.ms
(Source batch timeout)
`integer`
Maximum time in milliseconds to wait for a source batch to fill before processing. Alias for `processors.read_batch_timeout_ms`; takes priority when both are set.
Default: `100`
Minimum: `1`

connection.timeout.ms
(Source connection timeout)
`integer`
Connection timeout in milliseconds for the source Redis client.
Default: `2000`
Minimum: `1`

socket.timeout.ms
(Source socket timeout)
`integer`
Socket read/write timeout in milliseconds for the source Redis client.
Default: `2000`
Minimum: `1`

retry.max.attempts
(Source retry max attempts)
`integer`
Maximum number of retry attempts for failed source Redis operations.
Default: `5`
Minimum: `1`

retry.initial.delay.ms
(Source retry initial delay)
`integer`
Initial delay in milliseconds before the first retry of a failed source Redis operation.
Default: `100`
Minimum: `1`

retry.max.delay.ms
(Source retry max delay)
`integer`
Maximum delay in milliseconds between retry attempts for source Redis operations.
Default: `3000`
Minimum: `1`

retry.backoff.multiplier
(Source retry backoff multiplier)
`number`
Exponential backoff multiplier between retry attempts for source Redis operations.
Default: `2`
Minimum: `1`

Additional Properties

Name
Type
Description
Required

Additional Properties
`string`, `number`, `boolean`

Minimal Properties: 1
Example

```yaml
stream.name.pattern: data:*
discovery.interval.ms: 1000
batch.size: 2000
batch.timeout.ms: 100
connection.timeout.ms: 2000
socket.timeout.ms: 2000
retry.max.attempts: 5
retry.initial.delay.ms: 100
retry.max.delay.ms: 3000
retry.backoff.multiplier: 2
```

#### processors.advanced.target: Advanced target settings

Advanced configuration properties for the target Redis client and sink. Flink processor only.

Properties

Name
Type
Description
Required

batch.size
(Target sink batch size)
`integer`
Maximum number of records the target sink writes in a single batch. Alias for `processors.write_batch_size`; takes priority when both are set.
Default: `200`
Minimum: `1`

flush.interval.ms
(Target sink flush interval)
`integer`
Maximum time in milliseconds the target sink waits to fill a batch before flushing it to Redis.
Default: `100`
Minimum: `1`

connection.timeout.ms
(Target connection timeout)
`integer`
Connection timeout in milliseconds for the target Redis client.
Default: `2000`
Minimum: `1`

socket.timeout.ms
(Target socket timeout)
`integer`
Socket read/write timeout in milliseconds for the target Redis client.
Default: `2000`
Minimum: `1`

retry.max.attempts
(Target retry max attempts)
`integer`
Maximum number of retry attempts for failed target Redis operations. Alias for `processors.retry_max_attempts`; takes priority when both are set.
Default: `5`
Minimum: `1`

retry.initial.delay.ms
(Target retry initial delay)
`integer`
Initial delay in milliseconds before the first retry of a failed target Redis operation. Alias for `processors.retry_initial_delay_ms`; takes priority when both are set.
Default: `1000`
Minimum: `1`

retry.max.delay.ms
(Target retry max delay)
`integer`
Maximum delay in milliseconds between retry attempts for target Redis operations. Alias for `processors.retry_max_delay_ms`; takes priority when both are set.
Default: `10000`
Minimum: `1`

retry.backoff.multiplier
(Target retry backoff multiplier)
`number`
Exponential backoff multiplier between retry attempts for target Redis operations.
Default: `2`
Minimum: `1`

wait.enabled
(Target replica wait enabled)
`boolean`
When `true`, RDI verifies that each write has been replicated to the target database's replica shards before acknowledging it. Enable this only when target database replication is enabled and a healthy replica is available. Alias for `processors.wait_enabled`; takes priority when both are set.
Default: `false`

wait.write.timeout.ms
(Target replica wait timeout)
`integer`
Maximum time in milliseconds to wait for target replica write verification. Alias for `processors.wait_timeout`; takes priority when both are set.
Default: `1000`
Minimum: `1`

wait.retry.enabled
(Target replica wait retry enabled)
`boolean`
When `true`, RDI keeps retrying a target write until replica replication is confirmed; when `false`, it gives up after the first failure. Alias for `processors.retry_on_replica_failure`; takes priority when both are set. When enabled, the Flink processor retries indefinitely. Failed checkpoints can restart the job, after which the retries resume. The classic processor retries once.
Default: `true`

wait.retry.delay.ms
(Target replica wait retry delay)
`integer`
Delay in milliseconds between target replica wait retry attempts.
Default: `1000`
Minimum: `1`

Additional Properties

Name
Type
Description
Required

Additional Properties
`string`, `number`, `boolean`

Minimal Properties: 1
Example

```yaml
batch.size: 200
flush.interval.ms: 100
connection.timeout.ms: 2000
socket.timeout.ms: 2000
retry.max.attempts: 5
retry.initial.delay.ms: 1000
retry.max.delay.ms: 10000
retry.backoff.multiplier: 2
wait.enabled: false
wait.write.timeout.ms: 1000
wait.retry.enabled: true
wait.retry.delay.ms: 1000
```

#### processors.advanced.dlq: Advanced DLQ settings

Advanced configuration properties for the DLQ Redis client and sink. Flink processor only.

Properties

Name
Type
Description
Required

max.len
(DLQ sink max length)
`integer`
Maximum number of messages stored per dead letter queue stream. Alias for `processors.dlq_max_messages`; takes priority when both are set.
Default: `1000`
Minimum: `1`

batch.size
(DLQ sink batch size)
`integer`
Maximum number of records the DLQ sink writes in a single batch.
Default: `100`
Minimum: `1`

flush.interval.ms
(DLQ sink flush interval)
`integer`
Maximum time in milliseconds the DLQ sink waits to fill a batch before flushing it to Redis.
Default: `100`
Minimum: `1`

connection.timeout.ms
(DLQ connection timeout)
`integer`
Connection timeout in milliseconds for the DLQ Redis client.
Default: `2000`
Minimum: `1`

socket.timeout.ms
(DLQ socket timeout)
`integer`
Socket read/write timeout in milliseconds for the DLQ Redis client.
Default: `2000`
Minimum: `1`

retry.max.attempts
(DLQ retry max attempts)
`integer`
Maximum number of retry attempts for failed DLQ Redis operations.
Default: `1`
Minimum: `1`

retry.initial.delay.ms
(DLQ retry initial delay)
`integer`
Initial delay in milliseconds before the first retry of a failed DLQ Redis operation.
Default: `100`
Minimum: `1`

retry.max.delay.ms
(DLQ retry max delay)
`integer`
Maximum delay in milliseconds between retry attempts for DLQ Redis operations.
Default: `3000`
Minimum: `1`

retry.backoff.multiplier
(DLQ retry backoff multiplier)
`number`
Exponential backoff multiplier between retry attempts for DLQ Redis operations.
Default: `2`
Minimum: `1`

wait.enabled
(DLQ replica wait enabled)
`boolean`
When `true`, RDI verifies that each DLQ write has been replicated to the DLQ database's replica shards before acknowledging it.
Default: `false`

wait.write.timeout.ms
(DLQ replica wait timeout)
`integer`
Maximum time in milliseconds to wait for DLQ replica write verification.
Default: `1000`
Minimum: `1`

wait.retry.enabled
(DLQ replica wait retry enabled)
`boolean`
When `true`, RDI keeps retrying a DLQ write until replica replication is confirmed; when `false`, it gives up after the first failure.
Default: `false`

wait.retry.delay.ms
(DLQ replica wait retry delay)
`integer`
Delay in milliseconds between DLQ replica wait retry attempts.
Default: `1000`
Minimum: `1`

Additional Properties

Name
Type
Description
Required

Additional Properties
`string`, `number`, `boolean`

Minimal Properties: 1
Example

```yaml
max.len: 1000
batch.size: 100
flush.interval.ms: 100
connection.timeout.ms: 2000
socket.timeout.ms: 2000
retry.max.attempts: 1
retry.initial.delay.ms: 100
retry.max.delay.ms: 3000
retry.backoff.multiplier: 2
wait.enabled: false
wait.write.timeout.ms: 1000
wait.retry.enabled: false
wait.retry.delay.ms: 1000
```

#### processors.advanced.processor: Advanced processor settings

Advanced configuration properties for the processor. Flink processor only.

Properties

Name
Type
Description
Required

default.data.type
(Default target data type)
`string`
Data type to use in Redis when not overridden per job: `hash` for Redis Hash, `json` for RedisJSON. Alias for `processors.target_data_type`; takes priority when both are set.
Default: `"hash"`
Enum: `"hash"`, `"json"`

default.json.update.strategy
(Default JSON update strategy)
`string`
Strategy for updating JSON data in Redis: `replace` to overwrite the entire JSON object, `merge` to merge new data with the existing JSON object. Alias for `processors.json_update_strategy`; takes priority when both are set.
Default: `"replace"`
Enum: `"replace"`, `"merge"`

dlq.enabled
(Enable DLQ)
`boolean`
When `true`, rejected messages are stored in the dead-letter queue; when `false`, errors are silently skipped. Alias for `processors.error_handling`; takes priority when both are set.
Default: `true`

Additional Properties

Name
Type
Description
Required

Additional Properties
`string`, `number`, `boolean`

Minimal Properties: 1
Example

```yaml
default.data.type: hash
default.json.update.strategy: replace
dlq.enabled: true
```

#### processors.advanced.flink: Advanced Flink settings

Advanced configuration properties forwarded to the underlying Flink runtime. Any property listed in the [Flink configuration documentation](https://nightlies.apache.org/flink/flink-docs-release-2.0/docs/deployment/config/) can be set here and will override the RDI default. Flink processor only.The properties listed below are the ones most likely to require adjustment. Changing any other Flink property is not recommended unless instructed by Redis support.

Properties

Name
Type
Description
Required

parallelism.default
(Default parallelism)
`integer`
Default parallelism for jobs and operators. When unset, Flink uses the number of available task slots across all task managers (`taskManager.replicas Ã taskmanager.numberOfTaskSlots`). Increase to fan out work across more task slots; see [parallel execution](https://nightlies.apache.org/flink/flink-docs-release-2.0/docs/dev/datastream/execution/parallel/).
Minimum: `1`

taskmanager.numberOfTaskSlots
(Task slots per task manager)
`integer`
Number of parallel task slots per task manager pod. Each slot can run one parallel pipeline instance, so this caps the parallelism a single task manager can absorb. See [task slots and resources](https://nightlies.apache.org/flink/flink-docs-release-2.0/docs/concepts/flink-architecture/#task-slots-and-resources).
Default: `1`
Minimum: `1`

taskmanager.memory.process.size
(Task manager process memory)
`string`
Total memory budget for each task manager JVM process (heap + managed + network + metaspace + JVM overhead), expressed with a unit suffix such as `2048m` or `4g`. See [task manager memory configuration](https://nightlies.apache.org/flink/flink-docs-release-2.0/docs/deployment/memory/mem_setup_tm/).
Default: `"2048m"`

Additional Properties

Name
Type
Description
Required

Additional Properties
`string`, `number`, `boolean`

Minimal Properties: 1
Example

```yaml
taskmanager.numberOfTaskSlots: 1
taskmanager.memory.process.size: 2048m
```

#### processors.advanced.resources: Advanced resource settings

Compute resources allocated to the Flink job, such as the number of task manager pods. Flink processor only.

Properties

Name
Type
Description
Required

[taskManager](#processorsadvancedresourcestaskmanager)
(Task manager resource settings)
`object`
Resource settings for Flink task manager pods.

Additional Properties: not allowed
Minimal Properties: 1
Example

```yaml
taskManager: {}
```

##### processors.advanced.resources.taskManager: Task manager resource settings

Resource settings for Flink task manager pods.

Properties

Name
Type
Description
Required

replicas
(Task manager replicas)
`integer`
Number of Flink task manager pods to run.
Minimum: `1`

Additional Properties: not allowed
Minimal Properties: 1

## secret-providers: Secret providers

External secret providers used to resolve `${...}` references in the configuration.

Properties (key: `.*`)

Name
Type
Description
Required

type
(Provider type)
`string`
Secret provider backend. `aws` uses AWS Secrets Manager; `vault` uses HashiCorp Vault.
Enum: `"aws"`, `"vault"`

yes

[parameters](#secret-providersparameters)
(Provider parameters)
`object`
Configuration parameters for the secret provider.

yes

### secret-providers.parameters: Provider parameters

Configuration parameters for the secret provider.

Properties

Name
Type
Description
Required

[objects](#secret-providersparametersobjects)
(Secrets objects array)
`object[]`
Secret objects to fetch from the provider.

yes

Example

```yaml
objects:
  - {}
```

#### secret-providers.parameters.objects[]: Secrets objects array

Secret objects to fetch from the provider.

Items: Secret object

No properties.

Example

```yaml
- {}
```

## metadata: Pipeline metadata

Optional metadata describing this pipeline, such as a display name and description.

Properties

Name
Type
Description
Required

name
(Pipeline name)
`string`
Human-readable name for the pipeline. Maximum 100 characters.
Maximal Length: `100`

description
(Pipeline description)
`string`
Free-form description of what the pipeline does. Maximum 500 characters.
Maximal Length: `500`

revision
(Pipeline revision)
`integer`
Pipeline revision number. Must be a non-negative integer.
Minimum: `0`

[tags](#metadatatags)
(Pipeline tags)
`string[]`
Array of pipeline tags. Each tag must be a string of up to 50 characters, containing only alphanumeric characters, dots, dashes, or underscores, and must start and end with an alphanumeric character. Tags must be unique.

Additional Properties: not allowed

### metadata.tags[]: Pipeline tags

Array of pipeline tags. Each tag must be a string of up to 50 characters, containing only alphanumeric characters, dots, dashes, or underscores, and must start and end with an alphanumeric character. Tags must be unique.

Unique Items: yes

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page