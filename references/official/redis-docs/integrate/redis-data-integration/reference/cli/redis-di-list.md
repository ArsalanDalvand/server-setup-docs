---
title: "redis-di list"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-list/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-list"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di list"
---
# redis-di list

Lists all pipelines

Lists all pipelines with their status in a compact table.

## Usage

```
redis-di list [flags]
```

## Options

Option
Description

`-o`, `--output`
Output format: `table` (default), `json`, or `yaml`.

This command also accepts the
[[integrate/redis-data-integration/reference/cli/redis-di#global-options|global options]].

## Example

```bash
redis-di list
redis-di list -o json
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page