---
title: "Install Redis on macOS"
source: "https://redis.io/docs/latest/operate/oss_and_stack/install/archive/install-redis/install-redis-on-mac-os/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/oss_and_stack/install/archive/install-redis/install-redis-on-mac-os"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Install Redis on macOS"
---
# Install Redis on macOS

Use Homebrew to install and start Redis on macOS

            Redis Open Source
          

These docs apply to Redis Community Edition and Redis Stack version 7.x.

This guide shows you how to install Redis on macOS using Homebrew. Homebrew is the easiest way to install Redis on macOS. If you'd prefer to build Redis from the source files on macOS, see [[operate/oss_and_stack/install/archive/install-redis/install-redis-from-source|Installing Redis from Source]].

Note:
  The Homebrew distribution of Redis Open Source is only supported on macOS.

## Prerequisites

First, make sure you have Homebrew installed. From the terminal, run:

```bash
brew --version
```

If this command fails, you'll need to [follow the Homebrew installation instructions](https://brew.sh/).

## Installation

From the terminal, run:

```bash
brew install redis
```

This will install Redis on your system.

## Starting and stopping Redis in the foreground

To test your Redis installation, you can run the `redis-server` executable from the command line:

```bash
redis-server
```

If successful, you'll see the startup logs for Redis, and Redis will be running in the foreground.

To stop Redis, enter `Ctrl-C`.

### Starting and stopping Redis using launchd

As an alternative to running Redis in the foreground, you can also use `launchd` to start the process in the background:

```bash
brew services start redis
```

This launches Redis and restarts it at login. You can check the status of a `launchd` managed Redis by running the following:

```bash
brew services info redis
```

If the service is running, you'll see output like the following:

```bash
redis (homebrew.mxcl.redis)
Running: â
Loaded: â
User: miranda
PID: 67975
```

To stop the service, run:

```bash
brew services stop redis
```

## Connect to Redis

Once Redis is running, you can test it by running `redis-cli`:

```bash
redis-cli
```

Test the connection with the `ping` command:

```bash
127.0.0.1:6379> ping
PONG
```

You can also test that your Redis server is running using
[Redis Insight](/docs/latest/develop/tools/insight/).

## Next steps

Once you have a running Redis instance, you may want to:

Try the [[develop/tools/cli|Redis CLI tutorial]]
Connect using one of the [[develop/clients|Redis clients]]
[[operate/oss_and_stack/install/archive/install-redis#install-redis-properly|Install Redis "properly"]]
for production use.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page