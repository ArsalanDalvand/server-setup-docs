---
title: "redis-di delete-context"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-delete-context/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-delete-context"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di delete-context"
---
# redis-di delete-context

Deletes a context

Deletes a context from the `~/.redis-di` context file. Because this is destructive, the command asks
for confirmation unless you pass `--force`.

## Usage

```
redis-di delete-context <name> [flags]
```

## Options

Option
Description

`--force`
Skip the confirmation prompt.

This command also accepts the
[[integrate/redis-data-integration/reference/cli/redis-di#global-options|global options]].

## Example

```bash
redis-di delete-context dev --force
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page