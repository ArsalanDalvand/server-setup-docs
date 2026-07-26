---
title: "redis-di list-secrets"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-list-secrets/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-list-secrets"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di list-secrets"
---
# redis-di list-secrets

Lists the secrets of a pipeline

Lists the secrets of a pipeline. The API never returns secret values, so the output shows only the
secret keys and whether each one is set, not the stored values.

## Usage

```
redis-di list-secrets [flags]
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
redis-di list-secrets
redis-di list-secrets -p my-pipeline
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page