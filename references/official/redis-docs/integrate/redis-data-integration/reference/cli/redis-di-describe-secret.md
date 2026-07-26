---
title: "redis-di describe-secret"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-describe-secret/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-describe-secret"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di describe-secret"
---
# redis-di describe-secret

Describes a secret of a pipeline

Describes a single secret of a pipeline. The API never returns secret values, so the output shows
only the key and whether it is set, not the stored value.

## Usage

```
redis-di describe-secret <key> [flags]
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
redis-di describe-secret TARGET_DB_PASSWORD
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page