---
title: "redis-di describe-job"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-describe-job/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-describe-job"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di describe-job"
---
# redis-di describe-job

Describes a job of a pipeline

Describes a single job of a pipeline, printing its source properties followed by tables that
summarize its transformations and outputs.

## Usage

```
redis-di describe-job <name> [flags]
```

## Options

Option
Description

`-p`, `--pipeline`
Pipeline to target (default `default`).

This command also accepts the
[[integrate/redis-data-integration/reference/cli/redis-di#global-options|global options]].

## Example

```bash
redis-di describe-job customers_hash_job
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page