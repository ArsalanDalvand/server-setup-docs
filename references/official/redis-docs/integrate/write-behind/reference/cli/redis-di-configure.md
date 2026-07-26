---
title: "redis-di configure"
source: "https://redis.io/docs/latest/integrate/write-behind/reference/cli/redis-di-configure/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/write-behind/reference/cli/redis-di-configure"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "redis-di configure"
---
# redis-di configure

Configures the Write-behind Database so it is ready to process data

## Usage

```
Usage: redis-di configure [OPTIONS]
```

## Options

`loglevel`:

Type: Choice(['DEBUG', 'INFO', 'WARN', 'ERROR', 'CRITICAL'])
Default: `info`
Usage: `--loglevel -log-level`

`rdi_host` (REQUIRED):

Type: STRING
Default: `none`
Usage: `--rdi-host`

Host/IP of Write-behind Database

`rdi_port` (REQUIRED):

Type: <IntRange 1000<=x<=65535>
Default: `none`
Usage: `--rdi-port`

Port of Write-behind Database

`rdi_password`:

Type: STRING
Default: `none`
Usage: `--rdi-password`

Write-behind Database Password

`rdi_key`:

Type: STRING
Default: `none`
Usage: `--rdi-key`

Private key file to authenticate with

`rdi_cert`:

Type: STRING
Default: `none`
Usage: `--rdi-cert`

Client certificate file to authenticate with

`rdi_cacert`:

Type: STRING
Default: `none`
Usage: `--rdi-cacert`

CA certificate file to verify with

`rdi_key_password`:

Type: STRING
Default: `none`
Usage: `--rdi-key-password`

Password for unlocking an encrypted private key

`help`:

Type: BOOL
Default: `false`
Usage: `--help`

Show this message and exit.

## CLI help

```
Usage: redis-di configure [OPTIONS]

  Configures the Write-behind Database so it is ready to process data

Options:
  -log-level, --loglevel [DEBUG|INFO|WARN|ERROR|CRITICAL]
                                  [default: INFO]
  --rdi-host TEXT                 Host/IP of Write-behind Database  [required]
  --rdi-port INTEGER RANGE        Port of Write-behind Database  [1000<=x<=65535;
                                  required]
  --rdi-password TEXT             Write-behind Database Password
  --rdi-key TEXT                  Private key file to authenticate with
  --rdi-cert TEXT                 Client certificate file to authenticate with
  --rdi-cacert TEXT               CA certificate file to verify with
  --rdi-key-password TEXT         Password for unlocking an encrypted private
                                  key
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