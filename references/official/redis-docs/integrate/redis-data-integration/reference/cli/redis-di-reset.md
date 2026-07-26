---
title: "redis-di reset"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-reset/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-reset"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di reset"
---
# redis-di reset

Resets a pipeline

Resets a pipeline into initial full-sync mode, so it reloads a snapshot of the source data before
resuming change data capture. By default, the command waits for the pipeline to reach a terminal
state before returning.

## Usage

```
redis-di reset [pipeline] [flags]
```

The pipeline name is an optional argument that defaults to `default`.

## Options

Option
Description

`--wait`
Wait for the pipeline to reach the expected state (default `true`).

`--timeout`
Maximum time to wait for the pipeline to reach the expected state (default `2m`).

This command also accepts the
[[integrate/redis-data-integration/reference/cli/redis-di#global-options|global options]].

## Example

```bash
redis-di reset
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page