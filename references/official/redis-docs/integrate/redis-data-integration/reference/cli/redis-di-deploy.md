---
title: "redis-di deploy"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-deploy/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-deploy"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di deploy"
---
# redis-di deploy

Deploys a pipeline with the specified configuration

Deploys a pipeline, creating it or updating it from the configuration in the `--dir` directory. The
API validates the configuration and rejects an invalid one. By default, the command starts the
pipeline after deploying and waits for it to reach the expected state. `set` is an alias for this
command.

## Usage

```
redis-di deploy [pipeline] [flags]
```

The pipeline name is an optional argument that defaults to `default`.

## Options

Option
Description

`--dir`
Directory containing the pipeline configuration (default `.`).

`--dry-run`
Validate the configuration without deploying.

`--validate-tables`
Validate the configuration against the source and target databases (default `true`).

`--validate-cdc`
Validate the source database CDC configuration.

`--start`
Start the pipeline after deploying (default `true`).

`--wait`
Wait for the pipeline to reach the expected state (default `true`).

`--timeout`
Maximum time to wait for the pipeline to reach the expected state (default `2m`).

This command also accepts the
[[integrate/redis-data-integration/reference/cli/redis-di#global-options|global options]].

## Example

```bash
# Deploy the configuration in the current directory
redis-di deploy

# Validate a configuration folder without deploying it
redis-di deploy --dir /opt/rdi/config --dry-run
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page