---
title: "Install Redis Open Source on Linux"
source: "https://redis.io/docs/latest/operate/oss_and_stack/install/install-stack/snap/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/oss_and_stack/install/install-stack/snap"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Install Redis Open Source on Linux"
---
# Install Redis Open Source on Linux

How to install Redis Open Source using Snap

            Redis Open Source
          

## Install Redis Open Source on Ubuntu Linux using Snap

To install Redis via snap, run the following commands:

```bash
sudo apt update
sudo apt install redis-tools # this will install `redis-cli`
sudo snap install redis
```

Redis will start automatically after installation and also at boot time.

## Connect to Redis

Once Redis is running, you can test it by running `redis-cli`:

```bash
redis-cli
```

Test the connection with the `ping` command:

```bash
127.0.0.1:6379> PING
PONG
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page