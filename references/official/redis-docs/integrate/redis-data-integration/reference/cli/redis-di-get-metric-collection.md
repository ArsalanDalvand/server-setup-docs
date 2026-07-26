---
title: "redis-di get-metric-collection"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-get-metric-collection/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-get-metric-collection"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di get-metric-collection"
---
# redis-di get-metric-collection

Gets a metric collection of a pipeline

Gets a single metric collection of a pipeline, returning its raw metric data. This command is most
useful with `-o json` or `-o yaml` for scripting and for tools such as `jq`. Use
[[integrate/redis-data-integration/reference/cli/redis-di-list-metric-collections|`list-metric-collections`]]
to see the available collections.

## Usage

```
redis-di get-metric-collection <name> [flags]
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
redis-di get-metric-collection processor -o json
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page