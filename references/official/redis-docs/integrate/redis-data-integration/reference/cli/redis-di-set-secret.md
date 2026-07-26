---
title: "redis-di set-secret"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-set-secret/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-set-secret"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di set-secret"
---
# redis-di set-secret

Creates or updates a secret of a pipeline

Creates or updates a secret of a pipeline. Secrets hold the credentials and certificates that the
pipeline uses to connect to the source and target databases (see
[Set secrets](/docs/latest/integrate/redis-data-integration/data-pipelines/deploy/#set-secrets)
for the list of secret names). You can then refer to a secret in the `config.yaml` file with the
syntax `${SECRET_NAME}`.

The secret value comes from the `[value]` argument, the `--file` option, or the `--literal` option.
If you provide none of these on an interactive terminal, the command prompts for the value without
echoing it.

## Usage

```
redis-di set-secret <key> [value] [flags]
```

## Options

Option
Description

`-p`, `--pipeline`
Pipeline to target (default `default`).

`--file`
Read the secret value from the file at this path.

`--literal`
Use this literal string as the secret value.

`--wait`
Wait for the pipeline to reach the expected state (default `true`).

`--timeout`
Maximum time to wait for the pipeline to reach the expected state (default `2m`).

This command also accepts the
[[integrate/redis-data-integration/reference/cli/redis-di#global-options|global options]].

## Example

```bash
# Value from an argument
redis-di set-secret SOURCE_DB_USERNAME myuser

# Value from a file (for example, a certificate)
redis-di set-secret SOURCE_DB_CACERT --file /path/to/myca.crt

# Value read from an interactive prompt
redis-di set-secret SOURCE_DB_PASSWORD
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page