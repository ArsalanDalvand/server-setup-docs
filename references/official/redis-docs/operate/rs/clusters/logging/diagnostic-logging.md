---
title: "Diagnostic logging"
source: "https://redis.io/docs/latest/operate/rs/clusters/logging/diagnostic-logging/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/clusters/logging/diagnostic-logging"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Diagnostic logging"
---
# Diagnostic logging

Diagnostic logs can help troubleshoot Redis Software.

            Redis Software
          

The diagnostic logging service collects detailed system logs, which you can use to troubleshoot Redis Software.

## View diagnostic logs

Diagnostic logs are collected at scheduled intervals and saved in the `/var/opt/redislabs/log/diagnostics/` directory. Each diagnostic log file is overwritten with the new data at the scheduled collection interval.

## View log collector settings

To view the current log collection schedule and parameters for each log collector, use the REST API to [get the diagnostic logging service configuration](/docs/latest/operate/rs/references/rest-api/requests/diagnostics/#get-diagnostics).

```sh
GET /v1/diagnostics
```

Example response:

```json
{
    "bdb_client_list_target": {
        "cron_expression": "*/10 * * * *"
    },
    "bdb_info_target": {
        "cron_expression": "*/10 * * * *"
    },
    "bdb_target": {
        "cron_expression": "*/10 * * * *"
    },
    "command_stats_target": {
        "cron_expression": "*/30 * * * *"
    },
    "network_stats_target": {
        "cron_expression": "*/30 * * * *"
    },
    "persistent_files_target": {
        "cron_expression": "*/10 * * * *"
    },
    "rladmin_status_target": {
        "cron_expression": "*/10 * * * *"
    },
    "shard_info_target": {
        "cron_expression": "*/10 * * * *"
    },
    "shard_latency_histogram_target": {
        "cron_expression": "*/10 * * * *"
    },
    "shard_latency_target": {
        "cron_expression": "*/10 * * * *"
    },
    "shard_target": {
        "cron_expression": "*/10 * * * *"
    },
    "slowlog_target": {
        "cron_expression": "*/10 * * * *",
        "max_entries": 100
    },
    "socket_files_target": {
        "cron_expression": "*/10 * * * *"
    }
}
```

## Change log collector settings

To change how often a log is collected, set the `cron_expression` when you [update the diagnostic logging service configuration](/docs/latest/operate/rs/references/rest-api/requests/diagnostics/#put-diagnostics) with the REST API.

```sh
PUT /v1/diagnostics
{
  "<log_collector>_target": {
    "cron_expression": "*/5 * * * *"
  }
}
```

Replace `<log_collector>` with the name of the log collector you want to turn off.

Use standard [cron syntax](https://en.wikipedia.org/wiki/Cron) to set the collection interval in the `cron_expression`.

For the slowlog collector only, you can also set `max_entries` to change the maximum number of slow log entries to collect:

```sh
PUT /v1/diagnostics
{
  "slowlog_target": {
    "cron_expression": "*/5 * * * *",
    "max_entries": 200
  }
}
```

## Turn off log collectors

To turn off a log collector, set its `cron_expression` to an empty string when you [update the diagnostic logging service configuration](/docs/latest/operate/rs/references/rest-api/requests/diagnostics/#put-diagnostics) with the REST API.

```sh
PUT /v1/diagnostics
{
  "<log_collector>_target": {
    "cron_expression": ""
  }
}
```

Replace `<log_collector>` with the name of the log collector you want to turn off.

## Log collectors

Each log collector runs independently and writes a separate log file. The following table describes the log collectors.

Default `cron_expression` values:

`*/10 * * * *`: Logs are collected every 10 minutes.

`*/30 * * * *`: Logs are collected every 30 minutes.

Log collector
Description

bdb
Logs database metadata similar to [`GET /bdbs`](/docs/latest/operate/rs/references/rest-api/requests/bdbs/). Each entry is in JSON format.
Default settings:

```
"cron_expression": "*/10 * * * *"
```

bdb_client_list
Logs database client lists, with a separate file for each database. Each entry is in JSON format.
Default settings:

```
"cron_expression": "*/10 * * * *"
```

bdb_info
Logs the result of running [`INFO ALL`](/docs/latest/commands/info/) on a database, excluding `commandstats`, with a separate file for each database. Each entry is in JSON format.
Default settings:

```
"cron_expression": "*/10 * * * *"
```

command_stats
Logs [`INFO commandstats`](/docs/latest/commands/info/) for each database, with a separate file for each database. Each entry is in JSON format.
Default settings:

```
"cron_expression": "*/30 * * * *"
```

network_stats
Logs the node's network statistics.
Default settings:

```
"cron_expression": "*/30 * * * *"
```

persistent_files
Lists persistent files from `/var/opt/redislabs/persist/redis`
Default settings:

```
"cron_expression": "*/10 * * * *"
```

rladmin_status
Logs data about nodes, databases, endpoints, and shards from [`rladmin status`](/docs/latest/operate/rs/references/cli-utilities/rladmin/status/). Each entry is in JSON format.
Default settings:

```
"cron_expression": "*/10 * * * *"
```

shard
Logs shard status similar to [`GET /shards`](/docs/latest/operate/rs/references/rest-api/requests/shards/). Each entry is in JSON format.
Default settings:

```
"cron_expression": "*/10 * * * *"
```

shard_info
Logs the result of running [INFO ALL](/docs/latest/commands/info/) on a shard, with a separate file for each shard. Each entry is in JSON format.
Default settings:

```
"cron_expression": "*/10 * * * *"
```

shard_latency
Logs the result of running [`latency latest`](/docs/latest/commands/latency-latest/) on a shard, with a separate file for each shard.
Default settings:

```
"cron_expression": "*/10 * * * *"
```

`shard_latency_histogram`
Logs the result of running [`latency histogram`](/docs/latest/commands/latency-histogram/) on a shard, with a separate file for each shard.
Default settings:

```
"cron_expression": "*/10 * * * *"
```

slowlog
Logs slow commands from the databases using [`SLOWLOG GET`](/docs/latest/commands/slowlog-get/), with a separate file for each database. Each entry is in JSON format. The log is sanitized, only the commands are visible.
Default settings:

```
"cron_expression": "*/10 * * * *","max_entries": 100
```

socket_files
Lists socket files used by Redis Software.
Default settings:

```
"cron_expression": "*/10 * * * *"
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page