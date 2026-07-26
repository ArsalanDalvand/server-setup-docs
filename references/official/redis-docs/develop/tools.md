---
title: "Client tools"
source: "https://redis.io/docs/latest/develop/tools/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/tools"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Client tools"
---
# Client tools

Tools to interact with a Redis server

You can use several tools to connect to a Redis server, to
manage it and interact with the data:

The [`redis-cli`](#redis-command-line-interface-cli) command line tool
[Redis Insight](#redis-insight) (a graphical user interface tool)
The Redis [VSCode extension](#redis-vscode-extension)
[`redisctl`](#redisctl) (a unified CLI for managing Redis Cloud and Redis Software)

## Redis command line interface (CLI)

The [[develop/tools/cli|Redis command line interface]] (also known as `redis-cli`) is a terminal program that sends commands to and reads replies from the Redis server. It has the following two main modes:

An interactive Read Eval Print Loop (REPL) mode where the user types Redis commands and receives replies.
A command mode where `redis-cli` is executed with additional arguments, and the reply is printed to the standard output.

## Redis Insight

[Redis Insight](/docs/latest/develop/tools/insight/) combines a graphical user interface with Redis CLI to let you work with any Redis deployment. You can visually browse and interact with data, take advantage of diagnostic tools, learn by example, and much more. Best of all, Redis Insight is free.

[Download Redis Insight](https://redis.io/downloads/#insight).

## Redis VSCode extension

[Redis for VS Code](/docs/latest/develop/tools/redis-for-vscode/)
is an extension that allows you to connect to your Redis databases from within Microsoft Visual Studio Code. After connecting to a database, you can view, add, modify, and delete keys, and interact with your Redis databases using a Redis Insight like UI and also a built-in CLI interface.

## redisctl

[`redisctl`](https://github.com/redis/redisctl) is a unified command-line tool for managing Redis Cloud and Redis Software from your terminal. It provides complete API coverage for both platforms â including subscriptions, databases, VPC peering, ACLs, clusters, and users â without needing custom scripts. It also includes an MCP server component that exposes management operations to AI assistants.

Install via Homebrew, Cargo, or download a binary release from the [GitHub repository](https://github.com/redis/redisctl).

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page