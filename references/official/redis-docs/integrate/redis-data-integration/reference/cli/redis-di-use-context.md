---
title: "redis-di use-context"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-use-context/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-use-context"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di use-context"
---
# redis-di use-context

Sets a context to be the active one

Sets a context in the `~/.redis-di` context file to be the active one, so its connection details are
used by subsequent commands. Create or update a context with
[[integrate/redis-data-integration/reference/cli/redis-di-set-context|`set-context`]].

## Usage

```
redis-di use-context <name> [flags]
```

## Options

This command takes only the
[[integrate/redis-data-integration/reference/cli/redis-di#global-options|global options]].

## Example

```bash
redis-di use-context prod
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page