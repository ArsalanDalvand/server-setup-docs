---
title: "Using Redis commands"
source: "https://redis.io/docs/latest/develop/using-commands/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/using-commands"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Using Redis commands"
---
# Using Redis commands

Learn how to interact with Redis using commands.

Client applications and tools interact with Redis using commands. Most of the
commands implement [data types](/docs/latest/develop/data-types/) to store and retrieve data,
but there are also commands that deal with server configuration, security, and more.

The sections below give an overview of how Redis commands operate. See the
[[commands|Redis commands reference]] for a complete list of commands.

## Command structure

Each command is identified by a unique name. Related groups of commands
tend to follow a consistent naming convention. For example, all commands that
deal with [hashes](/docs/latest/develop/data-types/hashes/) start with the `H` prefix.
Most commands receive one or more arguments that specify the data to operate on.
For data type commands, the first argument is usually the [[develop/using-commands/keyspace|key]] that identifies the target data object.

After you issue a command, the server attempts to process it and then returns
a response. Commands that update data typically return a status message (such as `OK`)
or a number that indicates the number of items changed or updated. Commands that
retrieve data return the requested data. An unsuccessful command returns an
error message that describes the problem.

Interacting with a Redis server involves a sequence of commands and responses.
The effect of a given command is the same regardless of whether you send it
from a [[develop/clients|client library]], or from a client tool
such as [[develop/tools/cli|redis-cli]] or
[Redis Insight](/docs/latest/develop/tools/insight/). This is very useful
during development. You can use a high-level tool to experiment with a
command, set up test data, or prototype a data model, and then access the
prepared data from your application code. Most Redis code examples are
presented with an excerpt of a CLI session and the equivalent application code
for each client library.

## Batching commands

Although you can issue Redis commands one at a time, it's often more efficient
to batch a sequence of related commands together into a pipeline. A pipeline
sends several commands to the server as a single communication and receives
the responses in the same way. See
[[develop/using-commands/pipelining|Pipelining]] for a full
description of the technique and see also the pipelining examples for the
[[develop/clients|client libraries]].

Another reason to batch commands is to treat them as an uninterrupted unit.
You should do this if you need to be sure that the commands are all
completed without the same data being modified by another client (which
could leave the data in an inconsistent state). Redis uses transactions
to implement this behavior. See
[[develop/using-commands/transactions|Transactions]] for
more information and see also the transaction examples for the
[[develop/clients|client libraries]].

## More information

The other pages in this section describe Redis command concepts in more detail:

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page