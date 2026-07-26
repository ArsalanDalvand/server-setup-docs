---
title: "Install Write-behind CLI"
source: "https://redis.io/docs/latest/integrate/write-behind/installation/install-rdi-cli/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "integrate/write-behind/installation/install-rdi-cli"
category: "integrate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Install Write-behind CLI"
---
# Install Write-behind CLI

Install Write-behind CLI

The following installation instructions install the Write-behind CLI on a local workstation.

Write-behind installation is done via the Write-behind CLI. The CLI should have network access to the Redis Enterprise cluster API (port 9443 by default).

### Download Write-behind CLI

#### Ubuntu 20.04

```bash
wget https://qa-onprem.s3.amazonaws.com/redis-di/latest/redis-di-ubuntu20.04-latest.tar.gz -O /tmp/redis-di.tar.gz
```

#### Ubuntu 18.04

```bash
wget https://qa-onprem.s3.amazonaws.com/redis-di/latest/redis-di-ubuntu18.04-latest.tar.gz -O /tmp/redis-di.tar.gz
```

#### RHEL 8

```bash
wget https://qa-onprem.s3.amazonaws.com/redis-di/latest/redis-di-rhel8-latest.tar.gz -O /tmp/redis-di.tar.gz
```

#### RHEL 7

```bash
wget https://qa-onprem.s3.amazonaws.com/redis-di/latest/redis-di-rhel7-latest.tar.gz -O /tmp/redis-di.tar.gz
```

## Install Write-behind CLI

Unpack the downloaded `redis-di.tar.gz` into the `/usr/local/bin/` directory:

```bash
sudo tar xvf /tmp/redis-di.tar.gz -C /usr/local/bin/
```

Note: Non-root users should unpack to a directory with write permission and run `redis-di` directly from it.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page