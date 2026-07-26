---
title: "redis-di get-job"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-get-job/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-get-job"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di get-job"
---
# redis-di get-job

Gets a job of a pipeline

Gets a single job of a pipeline and prints it in the compact `list-jobs` table format. Use
[[integrate/redis-data-integration/reference/cli/redis-di-describe-job|`describe-job`]]
for the full job view with its transformations and outputs.

## Usage

```
redis-di get-job <name> [flags]
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
redis-di get-job customers_hash_job
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page