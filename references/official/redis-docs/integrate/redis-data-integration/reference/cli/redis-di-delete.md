---
title: "redis-di delete"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-delete/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-delete"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di delete"
---
# redis-di delete

Deletes a pipeline

Deletes a pipeline. Because this is destructive, the command asks for confirmation unless you pass
`--force`.

## Usage

```
redis-di delete [pipeline] [flags]
```

The pipeline name is an optional argument that defaults to `default`.

## Options

Option
Description

`--force`
Skip the confirmation prompt.

`--wait`
Wait for the pipeline to reach the expected state (default `true`).

`--timeout`
Maximum time to wait for the pipeline to reach the expected state (default `2m`).

This command also accepts the
[[integrate/redis-data-integration/reference/cli/redis-di#global-options|global options]].

## Example

```bash
redis-di delete my-pipeline --force
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page