---
title: "redis-di get-secret"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-get-secret/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-get-secret"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di get-secret"
---
# redis-di get-secret

Gets a secret of a pipeline

Gets a single secret of a pipeline and prints it in the compact `list-secrets` table format. The API
never returns secret values, so the output shows only the key and whether it is set, not the stored
value.

## Usage

```
redis-di get-secret <key> [flags]
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
redis-di get-secret SOURCE_DB_USERNAME
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page