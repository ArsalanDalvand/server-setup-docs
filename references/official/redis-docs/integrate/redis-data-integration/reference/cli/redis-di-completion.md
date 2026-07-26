---
title: "redis-di completion"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-completion/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-completion"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di completion"
---
# redis-di completion

Generates a shell autocompletion script

Generates an autocompletion script for `redis-di` for the specified shell. Supported shells are
`bash`, `zsh`, `fish`, and `powershell`.

## Usage

```
redis-di completion [bash|zsh|fish|powershell]
```

Run `redis-di completion <shell> --help` for the per-shell installation instructions.

## Example

To load completions into the current `bash` session:

```bash
source <(redis-di completion bash)
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page