---
title: "redis-di list-jobs"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-list-jobs/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-list-jobs"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di list-jobs"
---
# redis-di list-jobs

Lists the jobs of a pipeline

Lists the jobs of a pipeline, one row per job with its source, its transformation and output counts,
and the target connections of its outputs. Use
[[integrate/redis-data-integration/reference/cli/redis-di-describe-job|`describe-job`]]
for the full view of a single job.

## Usage

```
redis-di list-jobs [flags]
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
redis-di list-jobs
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page