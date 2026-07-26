---
title: "redis-di get"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-get/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-get"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di get"
---
# redis-di get

Gets a pipeline

Gets a single pipeline and prints it in the compact `list` table format. Use
[[integrate/redis-data-integration/reference/cli/redis-di-describe|`describe`]]
for the full pipeline view with its status and metrics.

## Usage

```
redis-di get [pipeline] [flags]
```

The pipeline name is an optional argument that defaults to `default`.

## Options

Option
Description

`-o`, `--output`
Output format: `table` (default), `json`, or `yaml`.

This command also accepts the
[[integrate/redis-data-integration/reference/cli/redis-di#global-options|global options]].

## Example

```bash
redis-di get
redis-di get my-pipeline -o yaml
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page