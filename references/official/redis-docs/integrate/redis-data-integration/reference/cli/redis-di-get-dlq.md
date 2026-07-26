---
title: "redis-di get-dlq"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-get-dlq/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-get-dlq"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di get-dlq"
---
# redis-di get-dlq

Gets a dead-letter queue of a pipeline

Gets a single dead-letter queue (DLQ) of a pipeline and prints it in the compact `list-dlqs` table
format.

## Usage

```
redis-di get-dlq <name> [flags]
```

## Options

Option
Description

`-p`, `--pipeline`
Pipeline to target (default `default`).

`-o`, `--output`
Output format: `table` (default), `json`, or `yaml`.

This command also accepts the
[[integrate/redis-data-integration/reference/cli/redis-di#global-options|global options]].

## Example

```bash
redis-di get-dlq inventory.customers
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page