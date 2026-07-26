---
title: "redis-di scaffold"
source: "https://redis.io/docs/latest/integrate/write-behind/reference/cli/redis-di-scaffold/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/write-behind/reference/cli/redis-di-scaffold"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di scaffold"
---
# redis-di scaffold

Generates configuration files for Write-behind and Debezium (when ingesting data to Redis)

## Usage

```
Usage: redis-di scaffold [OPTIONS]
```

## Options

`loglevel`:

Type: Choice(['DEBUG', 'INFO', 'WARN', 'ERROR', 'CRITICAL'])
Default: `info`
Usage: `--loglevel -log-level`

`db_type` (REQUIRED):

Type: Choice([<DbType.MYSQL: 'mysql'>, <DbType.ORACLE: 'oracle'>, <DbType.POSTGRESQL: 'postgresql'>, <DbType.REDIS: 'redis'>, <DbType.SQLSERVER: 'sqlserver'>])
Default: `none`
Usage: `--db-type`

DB type

`strategy`:

Type: Choice([<Strategy.INGEST: 'ingest'>, <Strategy.WRITE_BEHIND: 'write_behind'>])
Default: `ingest`
Usage: `--strategy`

Strategy

Output to directory or stdout

`directory`:

Type: STRING
Default: `none`
Usage: `--dir`

Directory containing Write-behind configuration

`preview`:

Type: Choice(['debezium/application.properties', 'config.yaml'])
Default: `none`
Usage: `--preview`

Print the content of specified config file to CLI output

`help`:

Type: BOOL
Default: `false`
Usage: `--help`

Show this message and exit.

## CLI help

```
Usage: redis-di scaffold [OPTIONS]

  Generates configuration files for Write-behind and Debezium (when ingesting data to
  Redis)

Options:
  -log-level, --loglevel [DEBUG|INFO|WARN|ERROR|CRITICAL]
                                  [default: INFO]
  --db-type [mysql|oracle|postgresql|redis|sqlserver]
                                  DB type  [required]
  --strategy [ingest|write_behind]
                                  Strategy  [default: ingest]
  Output formats: [mutually_exclusive, required]
                                  Output to directory or stdout
    --dir TEXT                    Directory containing Write-behind configuration
    --preview [debezium/application.properties|config.yaml]
                                  Print the content of specified config file
                                  to CLI output
  --help                          Show this message and exit.
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page