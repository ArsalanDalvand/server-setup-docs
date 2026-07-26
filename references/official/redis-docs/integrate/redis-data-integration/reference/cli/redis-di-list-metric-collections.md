---
title: "redis-di list-metric-collections"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-list-metric-collections/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-list-metric-collections"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di list-metric-collections"
---
# redis-di list-metric-collections

Lists the metric collections of a pipeline

Lists the metric collections of a pipeline. Metric collections hold the raw component metrics that
the [[integrate/redis-data-integration/reference/cli/redis-di-describe|`describe`]]
command summarizes in its Statistics and Performance sections. This command is most useful with
`-o json` or `-o yaml` for scripting and for tools such as `jq`.

## Usage

```
redis-di list-metric-collections [flags]
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
redis-di list-metric-collections
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page