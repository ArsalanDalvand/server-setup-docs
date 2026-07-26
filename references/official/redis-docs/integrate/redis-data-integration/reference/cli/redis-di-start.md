---
title: "redis-di start"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-start/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-start"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di start"
---
# redis-di start

Starts a pipeline

Starts a pipeline. By default, the command waits for the pipeline to reach the `started` state before
returning.

## Usage

```
redis-di start [pipeline] [flags]
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
redis-di start
redis-di start my-pipeline --wait=false
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page