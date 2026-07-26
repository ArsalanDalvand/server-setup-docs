---
title: "redis-di scaffold"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-scaffold/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-scaffold"
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

Generates pipeline configuration files

Generates a starter pipeline configuration for the given source database type. With `--dir`, the
command writes a `config.yaml` file into that directory, prompting before overwriting an existing
file unless `--force` is set. Without `--dir`, it prints the configuration to standard output.

## Usage

```
redis-di scaffold [flags]
```

## Options

Option
Description

`--db-type`
Source database type (required): `mariadb`, `mongodb`, `mysql`, `oracle`, `postgresql`, `snowflake`, `sqlserver`, or `spanner`.

`--db-flavor`
Source database flavor: `mongodb-atlas`, `mongodb-replica-set`, or `mongodb-sharded-cluster`.

`--dir`
Directory to write `config.yaml` to; prints to standard output when omitted.

`--force`
Skip the confirmation prompt when overwriting an existing file.

This command also accepts the
[[integrate/redis-data-integration/reference/cli/redis-di#global-options|global options]].

## Example

```bash
# Print a PostgreSQL configuration to stdout
redis-di scaffold --db-type postgresql

# Write a MySQL configuration into a directory
redis-di scaffold --db-type mysql --dir /opt/rdi/config
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page