---
title: "redis-di set-context"
source: "https://redis.io/docs/latest/integrate/write-behind/reference/cli/redis-di-set-context/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/write-behind/reference/cli/redis-di-set-context"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di set-context"
---
# redis-di set-context

Sets a context to be the active one

## Usage

```
Usage: redis-di set-context [OPTIONS] CONTEXT_NAME
```

## Options

`loglevel`:

Type: Choice(['DEBUG', 'INFO', 'WARN', 'ERROR', 'CRITICAL'])
Default: `info`
Usage: `--loglevel -log-level`

`context_name` (REQUIRED):

Type: STRING
Default: `none`
Usage: `context-name`

`help`:

Type: BOOL
Default: `false`
Usage: `--help`

Show this message and exit.

## CLI help

```
Usage: redis-di set-context [OPTIONS] CONTEXT_NAME

  Sets a context to be the active one

Options:
  -log-level, --loglevel [DEBUG|INFO|WARN|ERROR|CRITICAL]
                                  [default: INFO]
  --help                          Show this message and exit.
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page