---
title: "redis-di list-dlq-records"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-list-dlq-records/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-list-dlq-records"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di list-dlq-records"
---
# redis-di list-dlq-records

Lists the rejected records of a dead-letter queue

Lists the rejected records of a single dead-letter queue (DLQ), taking the queue name as an argument
and paging with `--limit`, `--offset`, and `--sort-order`. The operation code is shown by name
(create, update, delete, read). `get-rejected` is an alias for this command.

Use [[integrate/redis-data-integration/reference/cli/redis-di-list-dlqs|`list-dlqs`]]
to see all the pipeline's dead-letter queues and their record counts.

## Usage

```
redis-di list-dlq-records <dlq-name> [flags]
```

## Options

Option
Description

`-p`, `--pipeline`
Pipeline to target (default `default`).

`--limit`
Maximum number of records to return (default `20`).

`--offset`
Number of records to skip (default `0`).

`--sort-order`
Sort order: `asc` (oldest first) or `desc` (newest first, default).

`-o`, `--output`
Output format: `table` (default), `json`, or `yaml`.

The following options are kept for backward compatibility with the `get-rejected` command and are
deprecated:

Deprecated option
Use instead

`--dlq-name`
Pass the DLQ name as an argument.

`--max-records`
`--limit`

`--oldest`
`--sort-order asc`

This command also accepts the
[[integrate/redis-data-integration/reference/cli/redis-di#global-options|global options]].

## Example

```bash
# Newest 20 rejected records of a queue
redis-di list-dlq-records inventory.customers

# Oldest 100 records, as JSON
redis-di list-dlq-records inventory.customers --limit 100 --sort-order asc -o json
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page