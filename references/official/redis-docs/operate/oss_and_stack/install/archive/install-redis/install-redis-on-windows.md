---
title: "Install Redis on Windows"
source: "https://redis.io/docs/latest/operate/oss_and_stack/install/archive/install-redis/install-redis-on-windows/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/oss_and_stack/install/archive/install-redis/install-redis-on-windows"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Install Redis on Windows"
---
# Install Redis on Windows

Use Redis on Windows through Memurai, official Redis partner for Windows compatibility

            Redis Open Source
          

These docs apply to Redis Community Edition and Redis Stack version 7.x.

## Run Redis on Windows using Memurai

Redis is now natively supported on Windows through [Memurai](https://www.memurai.com/), the official Redis partner for Windows compatibility.

## Run Redis on Windows using WSL (Windows Subsystem for Linux)

To install Redis on Windows using WSL, you'll first need to enable [WSL2](https://docs.microsoft.com/en-us/windows/wsl/install) (Windows Subsystem for Linux). WSL2 lets you run Linux binaries natively on Windows. For this method to work, you'll need to be running Windows 10 version 2004 and higher or Windows 11.

### Install or enable WSL2

Microsoft provides [detailed instructions for installing WSL](https://docs.microsoft.com/en-us/windows/wsl/install). Follow these instructions, and take note of the default Linux distribution it installs. This guide assumes Ubuntu.

### Install Redis

Once you're running Ubuntu on Windows, you can follow the steps detailed at [[operate/oss_and_stack/install/archive/install-redis/install-redis-on-linux#install-on-ubuntu-debian|Install on Ubuntu/Debian]] to install recent stable versions of Redis from the official `packages.redis.io` APT repository.
Add the repository to the `apt` index, update it, and then install:

```bash
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list

sudo apt-get update
sudo apt-get install redis
```

Lastly, start the Redis server like so:

```bash
redis-server --daemonize yes
```

### Connect to Redis

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