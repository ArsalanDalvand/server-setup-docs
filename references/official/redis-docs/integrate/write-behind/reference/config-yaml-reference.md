---
title: "Write-behind configuration file"
source: "https://redis.io/docs/latest/integrate/write-behind/reference/config-yaml-reference/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/write-behind/reference/config-yaml-reference"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Write-behind configuration file"
---
# Write-behind configuration file

Write-behind configuration file reference

## Write-behind Configuration File

Properties

Name
Type
Description
Required

[applier](#applier)
(Configuration details of Write-behind Applier Gear)
`object`, `null`

[connections](#connections)
`object`

### applier: Configuration details of Write-behind Applier Gear

Properties

Name
Type
Description
Required

on_failed_retry_interval
(Interval (in seconds) on which to perform retry on failure)
`integer`, `string`
Default: `5`
Pattern: `^\${.*}$`
Minimum: `1`

read_batch_size
(The batch size for reading data from source database)
`integer`, `string`
Default: `2000`
Pattern: `^\${.*}$`
Minimum: `1`

debezium_lob_encoded_placeholder
(Enable Debezium LOB placeholders)
`string`
Default: `"X19kZWJleml1bV91bmF2YWlsYWJsZV92YWx1ZQ=="`

dedup
(Enable deduplication mechanism)
`boolean`
Default: `false`

dedup_max_size
(Max size of the deduplication set)
`integer`
Default: `1024`
Minimum: `1`

duration
(Time (in ms) after which data will be read from stream even if read_batch_size was not reached)
`integer`, `string`
Default: `100`
Pattern: `^\${.*}$`
Minimum: `1`

write_batch_size
(The batch size for writing data to target Redis database. Should be less or equal to the read_batch_size)
`integer`, `string`
Default: `200`
Pattern: `^\${.*}$`
Minimum: `1`

error_handling
(Error handling strategy: ignore - skip, dlq - store rejected messages in a dead letter queue)
`string`
Default: `"dlq"`
Pattern: ``^${.*}$
ignore
dlq``

dlq_max_messages
(Dead letter queue max messages per stream)
`integer`, `string`
Default: `1000`
Pattern: `^\${.*}$`
Minimum: `1`

target_data_type
(Target data type: hash/json - RedisJSON module must be in use in the target DB)
`string`
Default: `"hash"`
Pattern: ``^${.*}$
hash
json``

json_update_strategy
(Target update strategy: replace/merge - RedisJSON module must be in use in the target DB)
`string`
(DEPRECATED)
Property 'json_update_strategy' will be deprecated in future releases. Use 'on_update' job-level property to define the json update strategy.
Default: `"replace"`
Pattern: ``^${.*}$
replace
merge``

initial_sync_processes
(Number of processes Write-behind Engine creates to process the initial sync with the source)
`integer`, `string`
Default: `4`
Pattern: `^\${.*}$`
Minimum: `1`
Maximum: `32`

wait_enabled
(Checks if the data has been written to the replica shard of the target database)
`boolean`
Default: `false`

wait_timeout
(Timeout in milliseconds when checking write to the replica shard of the target database)
`integer`, `string`
Default: `1000`
Pattern: `^\${.*}$`
Minimum: `1`

retry_on_replica_failure
(Ensures that the data has been written to the replica shard and keeps retrying if not)
`boolean`
Default: `true`

Additional Properties: not allowed

### connections: Connections

Properties (Pattern)

Name
Type
Description
Required

.*

additionalProperties

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page