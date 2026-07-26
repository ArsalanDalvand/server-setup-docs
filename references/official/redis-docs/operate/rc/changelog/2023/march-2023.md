---
title: "Redis Cloud changelog (March 2023)"
source: "https://redis.io/docs/latest/operate/rc/changelog/2023/march-2023/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rc/changelog/2023/march-2023"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis Cloud changelog (March 2023)"
---
# Redis Cloud changelog (March 2023)

New features, enhancements, and other changes added to Redis Cloud during March 2023.

            Redis Cloud
          

This changelog lists new features, enhancements, and other changes added to Redis Cloud during March 2023.

## New features and enhancements

### Redis 7.0 preview

A preview of Redis 7.0 is available for [Fixed subscriptions](/docs/latest/operate/rc/databases/create-database/create-essentials-database/) in selected regions in AWS and GCP. However, some Redis 7.0 functionality might not be fully available during preview. Redis 7.0 also introduces several changes to existing Redis commands; see the [list of breaking changes](#redis-70-breaking-changes) for more details.

The following tables show which new open source Redis 7.0 commands are supported in Redis 7.0 subscriptions.

#### Cluster management commands

Command
Supported

[CLUSTER ADDSLOTSRANGE](/docs/latest/commands/cluster-addslotsrange/)
â Not supported

[CLUSTER DELSLOTSRANGE](/docs/latest/commands/cluster-delslotsrange/)
â Not supported

[CLUSTER LINKS](/docs/latest/commands/cluster-links/)
â Not supported

[CLUSTER SHARDS](/docs/latest/commands/cluster-shards/)
â Not supported

#### Connection management commands

Command
Supported

[CLIENT NO-EVICT](/docs/latest/commands/client-no-evict/)
â Not supported

#### Data type commands

Data type
Command
Supported

[[commands|List]]
[BLMPOP](/docs/latest/commands/blmpop/)
â Supported

[[commands|List]]
[LMPOP](/docs/latest/commands/lmpop/)
â Supported

[[commands|Set]]
[SINTERCARD](/docs/latest/commands/sintercard/)
â Supported

[[commands|Sorted set]]
[BZMPOP](/docs/latest/commands/bzmpop/)
â Supported

[[commands|Sorted set]]
[ZINTERCARD](/docs/latest/commands/zintercard/)
â Supported

[[commands|Sorted set]]
[BZMPOP](/docs/latest/commands/bzmpop/)
â Supported

#### Keys (generic) commands

Command
Supported

[EXPIRETIME](/docs/latest/commands/expiretime/)
â Supported

[PEXPIRETIME](/docs/latest/commands/pexpiretime/)
â Supported

[SORT_RO](/docs/latest/commands/sort_ro/)
â Supported

#### Pub/sub commands

Command
Supported

[PUBSUB SHARDCHANNELS](/docs/latest/commands/pubsub-shardchannels/)
â Supported

[PUBSUB SHARDNUMSUB](/docs/latest/commands/pubsub-shardnumsub/)
â Supported

[SPUBLISH](/docs/latest/commands/spublish/)
â Not supported

[SSUBSCRIBE](/docs/latest/commands/ssubscribe/)
â Not supported

[SUNSUBSCRIBE](/docs/latest/commands/sunsubscribe/)
â Not supported

#### Scripting and function commands

Command
Supported

[EVAL_RO](/docs/latest/commands/eval_ro/)
â Not supported

[EVALSHA_RO](/docs/latest/commands/evalsha_ro/)
â Not supported

[FUNCTION DELETE](/docs/latest/commands/function-delete/)
â Supported

[FUNCTION DUMP](/docs/latest/commands/function-dump/)
â Supported

[FUNCTION FLUSH](/docs/latest/commands/function-flush/)
â Supported

[FUNCTION HELP](/docs/latest/commands/function-help/)
â Supported

[FUNCTION KILL](/docs/latest/commands/function-kill/)
â Supported

[FUNCTION LIST](/docs/latest/commands/function-list/)
â Supported

[FUNCTION LOAD](/docs/latest/commands/function-load/)
â Supported

[FUNCTION RESTORE](/docs/latest/commands/function-restore/)
â Supported

[FUNCTION STATS](/docs/latest/commands/function-stats/)
â Not supported

#### Server management commands

Command
Supported

[ACL DRYRUN](/docs/latest/commands/acl-dryrun/)
â Not supported

[COMMAND DOCS](/docs/latest/commands/command-docs/)
â Supported

[COMMAND GETKEYSANDFLAGS](/docs/latest/commands/command-getkeysandflags/)
â Supported

[COMMAND LIST](/docs/latest/commands/command-list/)
â Supported

[MODULE LOADEX](/docs/latest/commands/module-loadex/)
â Not supported

[LATENCY HISTOGRAM](/docs/latest/commands/latency-histogram/)
â Not supported

## Breaking changes

### Redis 7.0 breaking changes

When new major versions of open source Redis change existing commands, upgrading your database to a new version can potentially break some functionality. Before you upgrade, make sure to read the provided list of breaking changes that affect Redis Enterprise and update any applications that connect to your database to handle these changes.

To check your Redis database version (`redis_version`), you can use the admin console or run the [`INFO`](/docs/latest/commands/info/) command with [`redis-cli`](/docs/latest/operate/rs/references/cli-utilities/redis-cli/):

```sh
$ redis-cli -p <port> INFO
"# Server
redis_version:7.0.8
..."
```

Open source Redis version 7.0 introduces the following potentially breaking changes to Redis Enterprise:

#### Programmability

Lua scripts no longer have access to the `print()` function ([#10651](https://github.com/redis/redis/pull/10651)) - The `print`  function was removed from Lua because it can potentially cause the Redis processes to get stuck (if no one reads from stdout). Users should use redis.log. An alternative is to override the  `print`  implementation and print the message to the log file.

Block [`PFCOUNT`](/docs/latest/commands/pfcount/) and [`PUBLISH`](/docs/latest/commands/publish/) in read-only scripts (*_RO commands,  `no-writes`) ([#10744](https://github.com/redis/redis/pull/10744)) - Consider `PFCOUNT` and `PUBLISH` as write commands in scripts, in addition to `EVAL`; meaning:

They can never be used in scripts with shebang (`#!`) and no `no-writes` flag
They are blocked in `EVAL_RO` and `_RO` variants, (even in scripts without shebang (`#!`) flags)
Allow `no-write` scripts in EVAL (not just in EVAL_RO), even during `CLIENT PAUSE WRITE`

Hide the `may_replicate` flag from the [`COMMAND`](/docs/latest/commands/command/) command response  ([#10744](https://github.com/redis/redis/pull/10744)) - As part of the change to treat `may_replicate` commands `PFCOUNT` and `PUBLISH` as write commands in scripts, in addition to `EVAL`, the `may_replicate` flag has been removed from the `COMMAND` response.

#### Error handling

Rephrased some error responses about invalid commands or arguments ([#10612](https://github.com/redis/redis/pull/10612)) -

Error response for unknown command introduced a case change (`Unknown` to `unknown`)
Errors for module commands extended to cover subcommands, updated syntax to match Redis Server syntax
Arity errors for module commands introduce a case change (`Wrong` to `wrong`); will consider full command name

Corrected error codes returned from [`EVAL`](/docs/latest/commands/eval/) scripts ([#10218](https://github.com/redis/redis/pull/10218), [#10329](https://github.com/redis/redis/pull/10329)).

These examples show changes in behavior:

```diff
1: config set maxmemory 1
  2: +OK
  3: eval "return redis.call('set','x','y')" 0
- 4: -ERR Error running script (call to 71e6319f97b0fe8bdfa1c5df3ce4489946dda479): @user_script:1: @user_script: 1: -OOM command not allowed when used memory > 'maxmemory'.
+ 4: -ERR Error running script (call to 71e6319f97b0fe8bdfa1c5df3ce4489946dda479): @user_script:1: OOM command not allowed when used memory > 'maxmemory'.
  5: eval "return redis.pcall('set','x','y')" 0
- 6: -@user_script: 1: -OOM command not allowed when used memory > 'maxmemory'.
+ 6: -OOM command not allowed when used memory > 'maxmemory'.
  7: eval "return redis.call('select',99)" 0
  8: -ERR Error running script (call to 4ad5abfc50bbccb484223905f9a16f09cd043ba8): @user_script:1: ERR DB index is out of range
  9: eval "return redis.pcall('select',99)" 0
 10: -ERR DB index is out of range
 11: eval_ro "return redis.call('set','x','y')" 0
-12: -ERR Error running script (call to 71e6319f97b0fe8bdfa1c5df3ce4489946dda479): @user_script:1: @user_script: 1: Write commands are not allowed from read-only scripts.
+12: -ERR Error running script (call to 71e6319f97b0fe8bdfa1c5df3ce4489946dda479): @user_script:1: ERR Write commands are not allowed from read-only scripts.
 13: eval_ro "return redis.pcall('set','x','y')" 0
-14: -@user_script: 1: Write commands are not allowed from read-only scripts.
+14: -ERR Write commands are not allowed from read-only scripts.
```

[`ZPOPMIN`](/docs/latest/commands/zpopmin/)/[`ZPOPMAX`](/docs/latest/commands/zpopmax/) used to produce wrong replies when count is 0 with non-zset  [#9711](https://github.com/redis/redis/pull/9711)):

`ZPOPMIN`/`ZPOPMAX` used to produce an `(empty array)` when `key` was not a sorted set and the optional `count` argument was set to `0` and now produces a `WRONGTYPE` error response instead.
The optional `count` argument must be positive. A negative value produces a `value is out of range` error.

These examples show changes in behavior:

```diff
1: zadd myzset 1 "one"
  2: (integer) 1
  3: zadd myzset 2 "two"
  4: (integer) 1
  5: zadd myzset 3 "three"
  6: (integer) 1
  7: zpopmin myzset -1
- 8: (empty array)
+ 8: (error) ERR value is out of range, must be positive
  9: 127.0.0.1:6379> set foo bar
 10: OK
 11: zpopmin foo 0
-12: (empty array)
+12: (error) WRONGTYPE Operation against a key holding the wrong kind of value
```

[`LPOP`](/docs/latest/commands/lpop/)/[`RPOP`](/docs/latest/commands/rpop/) with count against a nonexistent list returns a null array instead of `(nil)`([#10095](https://github.com/redis/redis/pull/10095)). This change was backported to 6.2.

[`LPOP`](/docs/latest/commands/lpop/)/[`RPOP`](/docs/latest/commands/rpop/) used to produce `(nil)` when count is 0, now produces a null array ([#9692](https://github.com/redis/redis/pull/9692)). This change was backported to 6.2.

[`XCLAIM`](/docs/latest/commands/xclaim/)/[`XAUTOCLAIM`](/docs/latest/commands/xautoclaim/) skips deleted entries instead of replying with `nil` and deletes them from the pending entry list ([#10227](https://github.com/redis/redis/pull/10227)) - `XCLAIM`/`XAUTOCLAIM` now behaves in the following way:

If you try to claim a deleted entry, it is deleted from the pending entry list (PEL) where it is found (as well as the group PEL). Therefore, such an entry is not claimed, just cleared from PEL (because it doesn't exist in the stream anyway).
Because deleted entries are not claimed, `X[AUTO]CLAIM` does not return "nil" instead of an entry.
Added an array of all the deleted stream IDs to `XAUTOCLAIM` response.

#### ACLs

[`ACL GETUSER`](/docs/latest/commands/acl-getuser/) reply now uses ACL syntax for `keys` and `channels` ([#9974](https://github.com/redis/redis/pull/9974)). `ACL GETUSER` now uses the ACL DSL (Domain Specific Language) for keys and channels.

These examples show changes in behavior:

```diff
1: acl setuser foo off resetchannels &channel1 -@all +get
  2: OK
  3: acl getuser foo
  4: 1) "flags"
  5: 2) 1) "off"
  6: 3) "passwords"
  7: 4) (empty array)
  8: 5) "commands"
  9: 6) "-@all +get"
 10: 7) "keys"
-11: 8) (empty array)
+11: 8) ""
 12: 9)"channels"
-13 10) 1) "channel1"
+13 10) "&channel1"
```

[`SORT`](/docs/latest/commands/sort/)/[`SORT_RO`](/docs/latest/commands/sort_ro/) commands reject key access patterns in `GET` and `BY` if the ACL doesn't grant the command full keyspace access ([#10340](https://github.com/redis/redis/pull/10340)) - The `sort` and `sort_ro` commands can access external keys via `GET` and `BY`. In order to make sure the user cannot violate the authorization ACL rules, Redis 7 will reject external keys access patterns unless ACL allows `SORT` full access to all keys.
For backwards compatibility, `SORT` with `GET`/`BY` keeps working, but if ACL has restrictions to certain keys, the use of these features will result in a permission denied error.

These examples show changes in behavior:

```
USER FOO (+sort ~* ~mylist) 
#FOO> sort mylist by w* get v*  - is O.K since ~* provides full key access
```

```
USER FOO (+sort %R~* ~mylist) 
#FOO> sort mylist by w* get v*  - is O.K since %R~* provides full key READ access**
```

```
USER FOO (+sort %W~* ~mylist)
#FOO> sort mylist by w* get v*  - will now fail since $W~* only provides full key WRITE access
```

```
USER FOO (+sort ~v* ~mylist)
#FOO> sort mylist by w* get v*  - will now fail since ~v* only provides partial key access
```

Fix ACL category for [`SELECT`](/docs/latest/commands/select/), [`WAIT`](/docs/latest/commands/wait/, [`ROLE`](/docs/latest/commands/role/) [`LASTSAVE`](/docs/latest/commands/lastsave/, [`READONLY`](/docs/latest/commands/readonly/, [`READWRITE`](/docs/latest/commands/readwrite/, [`ASKING`](/docs/latest/commands/asking/)  ([#9208](https://github.com/redis/redis/pull/9208)):

`SELECT` and `WAIT` have been recategorized from `@keyspace` to `@connection`
`ROLE`, `LASTSAVE` have been categorized as  `@admin`  and  `@dangerous`
`ASKING`, `READONLY`, `READWRITE` have also been assigned the  `@connection`  category and  removed from `@keyspace`
Command categories are explained in [[operate/oss_and_stack/management/security/acl#command-categories|ACL documentation]]

#### Command introspection, stats, and configuration

[`COMMAND`](/docs/latest/commands/command/) reply drops `random` and `sort-for-scripts` flags, which are now part of [[develop/reference/command-tips|command tips]] ([#10104](https://github.com/redis/redis/pull/10104)) - The `random` flag was replaced with the `nondeterministic_output` tip; the `sort-for-scripts` flag was replaced by the `nondeterministic_output_order` tip

[`INFO`](/docs/latest/commands/info/) `commandstats` now shows the stats per sub-command ([#9504](https://github.com/redis/redis/pull/9504))
For example, while previous versions would provide a single entry for all command usage, in Redis 7, each sub command is reported separately:

Redis 6.2:

```
cmdstat_acl:calls=4,usec=279,usec_per_call=69.75,rejected_calls=0,failed_calls=2
```

Redis 7:

```
cmdstat_acl|list:calls=1,usec=4994,usec_per_call=4994.00,rejected_calls=0,failed_calls=0
cmdstat_acl|setuser:calls=2,usec=16409,usec_per_call=8204.50,rejected_calls=0,failed_calls=0
cmdstat_acl|deluser:calls=1,usec=774,usec_per_call=774.00,rejected_calls=0,failed_calls=0
cmdstat_acl|getuser:calls=1,usec=6044,usec_per_call=6044.00,rejected_calls=0,failed_calls=0
```

[`CONFIG REWRITE`](/docs/latest/commands/config-rewrite/), [`CONFIG RESETSTAT`](/docs/latest/commands/config-resetstat/, and most [`CONFIG SET`](/docs/latest/commands/config-set/commands are now allowed during loading ([#9878](https://github.com/redis/redis/pull/9878))

## Deprecations

[`CLUSTER SLOTS`](/docs/latest/commands/cluster-slots/) is deprecated as of Redis 7.0

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page