---
title: "redis-di list-dlqs"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-list-dlqs/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-list-dlqs"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di list-dlqs"
---
# redis-di list-dlqs

Lists the dead-letter queues of a pipeline

Lists the dead-letter queues (DLQs) of a pipeline with their record counts. A DLQ holds the records
that RDI rejected. Use
[[integrate/redis-data-integration/reference/cli/redis-di-list-dlq-records|`list-dlq-records`]]
to read the records of a single queue.

## Usage

```
redis-di list-dlqs [flags]
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
redis-di list-dlqs
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page