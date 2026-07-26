---
title: "redis-di configure-rdi"
source: "https://redis.io/docs/latest/integrate/redis-data-integration/reference/cli/redis-di-configure-rdi/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/redis-data-integration/reference/cli/redis-di-configure-rdi"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di configure-rdi"
---
# redis-di configure-rdi

Configures the RDI database connection

Configures the connection credentials for the RDI database. This is an administration command that is
available only on VM installations, where `redis-di` forwards it to the bundled `rdi-admin` tool.

## Usage

```
redis-di configure-rdi [OPTIONS]
```

## Options

Option
Description

`-l`, `--log-level`
Log level: `TRACE`, `DEBUG`, `INFO`, `WARNING`, `ERROR`, or `CRITICAL` (default `INFO`).

`--rdi-namespace`
RDI Kubernetes namespace (default `rdi`).

`--rdi-host`
Host or IP of the RDI database (required).

`--rdi-port`
Port of the RDI database, `1`â`65535` (required).

`--rdi-user`
RDI database username.

`--rdi-password`
RDI database password.

`--rdi-key`
Private key file to authenticate with.

`--rdi-cert`
Client certificate file to authenticate with.

`--rdi-cacert`
CA certificate file to verify with.

`--rdi-key-password`
Password for unlocking an encrypted private key.

`--rdi-log-level`
Log level for the RDI components.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page