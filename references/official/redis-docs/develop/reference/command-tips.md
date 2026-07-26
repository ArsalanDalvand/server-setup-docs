---
title: "Redis command tips"
source: "https://redis.io/docs/latest/develop/reference/command-tips/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/reference/command-tips"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis command tips"
---
# Redis command tips

Get additional information about a command

Command tips are an array of strings.
These provide Redis clients with additional information about the command.
The information can instruct Redis Cluster clients as to how the command should be executed and its output processed in a clustered deployment.

Unlike the command's flags (see the 3rd element of [`COMMAND`](/docs/latest/commands/command/)'s reply), which are strictly internal to the server's operation, tips don't serve any purpose other than being reported to clients.

Command tips are arbitrary strings.
However, the following sections describe proposed tips and demonstrate the conventions they are likely to adhere to.

## nondeterministic_output

This tip indicates that the command's output isn't deterministic.
That means that calls to the command may yield different results with the same arguments and data.
That difference could be the result of the command's random nature (e.g., [`RANDOMKEY`](/docs/latest/commands/randomkey/) and [`SPOP`](/docs/latest/commands/spop/)); the call's timing (e.g., [`TTL`](/docs/latest/commands/ttl/)); or generic differences that relate to the server's state (e.g., [`INFO`](/docs/latest/commands/info/) and [`CLIENT LIST`](/docs/latest/commands/client-list/)).

Note:
Prior to Redis 7.0, this tip was the random command flag.

## nondeterministic_output_order

The existence of this tip indicates that the command's output is deterministic, but its ordering is random (e.g., [`HGETALL`](/docs/latest/commands/hgetall/) and [`SMEMBERS`](/docs/latest/commands/smembers/)).

Note:
Prior to Redis 7.0, this tip was the sort_for_script flag.

## request_policy

This tip can help clients determine the shards to send the command in clustering mode.
The default behavior a client should implement for commands without the request_policy tip is as follows:

The command doesn't accept key name arguments: the client can execute the command on an arbitrary shard.
For commands that accept one or more key name arguments: the client should route the command to a single shard, as determined by the hash slot of the input keys.

In cases where the client should adopt a behavior different than the default, the request_policy tip can be one of:

all_nodes: the client should execute the command on all nodes - masters and replicas alike.
An example is the [`CONFIG SET`](/docs/latest/commands/config-set/) command.
This tip is in-use by commands that don't accept key name arguments.
The command operates atomically per shard.

all_shards: the client should execute the command on all master shards (e.g., the [`DBSIZE`](/docs/latest/commands/dbsize/) command).
This tip is in-use by commands that don't accept key name arguments.
The command operates atomically per shard.

multi_shard: the client should execute the command on several shards.
The client should split the inputs according to the hash slots of its input key name arguments.
For example, the command `DEL {foo} {foo}1 bar` should be split to `DEL {foo} {foo}1` and `DEL bar`.
If the keys are hashed to more than a single slot, the command must be split even if all the slots are managed by the same shard.
Examples for such commands include [`MSET`](/docs/latest/commands/mset/), [`MGET`](/docs/latest/commands/mget/) and [`DEL`](/docs/latest/commands/del/).
However, note that [`SUNIONSTORE`](/docs/latest/commands/sunionstore/) isn't considered as multi_shard because all of its keys must belong to the same hash slot.
special: indicates a non-trivial form of the client's request policy, such as the [`SCAN`](/docs/latest/commands/scan/) command.

## response_policy

This tip can help clients determine the aggregate they need to compute from the replies of multiple shards in a cluster.
The default behavior for commands without a request_policy tip only applies to replies with of nested types (i.e., an array, a set, or a map).
The client's implementation for the default behavior should be as follows:

The command doesn't accept key name arguments: the client can aggregate all replies within a single nested data structure.
For example, the array replies we get from calling [`KEYS`](/docs/latest/commands/keys/) against all shards.
These should be packed in a single in no particular order.
For commands that accept one or more key name arguments: the client needs to retain the same order of replies as the input key names.
For example, [`MGET`](/docs/latest/commands/mget/)'s aggregated reply.

The response_policy tip is set for commands that reply with scalar data types, or when it's expected that clients implement a non-default aggregate.
This tip can be one of:

one_succeeded: the clients should return success if at least one shard didn't reply with an error.
The client should reply with the first non-error reply it obtains.
If all shards return an error, the client can reply with any one of these.
For example, consider a [`SCRIPT KILL`](/docs/latest/commands/script-kill/) command that's sent to all shards.
Although the script should be loaded in all of the cluster's shards, the [`SCRIPT KILL`](/docs/latest/commands/script-kill/) will typically run only on one at a given time.
all_succeeded: the client should return successfully only if there are no error replies.
Even a single error reply should disqualify the aggregate and be returned.
Otherwise, the client should return one of the non-error replies.
As an example, consider the [`CONFIG SET`](/docs/latest/commands/config-set/), [`SCRIPT FLUSH`](/docs/latest/commands/script-flush/) and [`SCRIPT LOAD`](/docs/latest/commands/script-load/) commands.
agg_logical_and: the client should return the result of a logical AND operation on all replies (only applies to integer replies, usually from commands that return either 0 or 1).
Consider the [`SCRIPT EXISTS`](/docs/latest/commands/script-exists/) command as an example.
It returns an array of 0's and 1's that denote the existence of its given SHA1 sums in the script cache.
The aggregated response should be 1 only when all shards had reported that a given script SHA1 sum is in their respective cache.
agg_logical_or: the client should return the result of a logical OR operation on all replies (only applies to integer replies, usually from commands that return either 0 or 1).
agg_min: the client should return the minimal value from the replies (only applies to numerical replies).
The aggregate reply from a cluster-wide [`WAIT`](/docs/latest/commands/wait/) command, for example, should be the minimal value (number of synchronized replicas) from all shards.
agg_max: the client should return the maximal value from the replies (only applies to numerical replies).
agg_sum: the client should return the sum of replies (only applies to numerical replies).
Example: [`DBSIZE`](/docs/latest/commands/dbsize/).
special: this type of tip indicates a non-trivial form of reply policy.
[`INFO`](/docs/latest/commands/info/) is an excellent example of that.

## Example

```
redis> command info ping
1)  1) "ping"
    2) (integer) -1
    3) 1) fast
    4) (integer) 0
    5) (integer) 0
    6) (integer) 0
    7) 1) @fast
       2) @connection
    8) 1) "request_policy:all_shards"
       2) "response_policy:all_succeeded"
    9) (empty array)
   10) (empty array)
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page