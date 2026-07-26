---
title: "Overview of Redis ACLs in Redis Software"
source: "https://redis.io/docs/latest/operate/rs/security/access-control/redis-acl-overview/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/security/access-control/redis-acl-overview"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Overview of Redis ACLs in Redis Software"
---
# Overview of Redis ACLs in Redis Software

An overview of Redis ACLs, syntax, and ACL command support in Redis Software.

            Redis Software
          

Redis access control lists (Redis ACLs) allow you to define named permissions for specific Redis commands, keys, and pub/sub channels. You can use defined Redis ACLs for multiple databases and roles.

## Predefined Redis ACLs

Redis Software provides one predefined Redis ACL named Full Access. This ACL allows all commands on all keys and cannot be edited.

## Redis ACL syntax

Redis ACLs are defined by a set of rules where you specify the commands or command categories that are allowed for specific keys.

### Commands and categories

Redis ACL rules can allow or block specific [[commands|Redis commands]] or [[operate/oss_and_stack/management/security/acl#command-categories|command categories]].

`+` includes commands

`-` excludes commands

`+@` includes command categories

`-@` excludes command categories

The following example allows all `read` commands and the `SET` command:

```sh
+@read +SET
```

For database versions earlier than Redis 8.2, module commands have several ACL limitations:

[Redis modules](/docs/latest/operate/oss_and_stack/stack-with-enterprise/) do not have command categories.

Other [[operate/oss_and_stack/management/security/acl#command-categories|command category]] ACLs, such as `+@read` and `+@write`, do not include Redis module commands. `+@all` is the only exception because it allows all Redis commands.

You have to include individual module commands in a Redis ACL rule to allow them.

For example, the following Redis ACL rule allows read-only commands and the Redis Search commands `FT.INFO` and `FT.SEARCH`:

```sh
+@read +FT.INFO +FT.SEARCH
```

### Key patterns

To define access to specific keys or key patterns, use the following prefixes:

`~` or `%RW~` allows read and write access to keys.

`%R~` allows read access to keys.

`%W~` allows write access to keys.

`%RW~`, `%R~`, and `%W~` are only supported for databases with Redis version 7.2 or later.

The following example allows read and write access to all keys that start with "app1" and read-only access to all keys that start with "app2":

```sh
~app1* %R~app2*
```

### Pub/sub channels

The `&` prefix allows access to [[develop/pubsub|pub/sub channels]] (only supported for databases with Redis version 6.2 or later).

To limit access to specific channels, include `resetchannels` before the allowed channels:

```sh
resetchannels &channel1 &channel2
```

### Selectors

[[operate/oss_and_stack/management/security/acl#selectors|Selectors]] let you define multiple sets of rules in a single Redis ACL (only supported for databases with Redis version 7.2 or later). A command is allowed if it matches the base rule or any selector in the Redis ACL.

`(<rule set>)` creates a new selector.

`clearselectors` deletes all existing selectors for a user. This action does not delete the base ACL rule.

In the following example, the base rule allows `GET key1` and the selector allows `SET key2`:

```sh
+GET ~key1 (+SET ~key2)
```

## Default pub/sub permissions

Redis database version 6.2 introduced pub/sub ACL rules that determine which [[develop/pubsub|pub/sub channels]] a user can access.

The configuration option `acl-pubsub-default`, added in Redis Software version 6.4.2, determines the cluster-wide default level of access for all pub/sub channels. Redis Software uses the following pub/sub permissions by default:

For versions 6.4.2 and 7.2, `acl-pubsub-default` is permissive (`allchannels` or `&*`) by default to accommodate earlier Redis versions.

In future versions, `acl-pubsub-default` will change to restrictive (`resetchannels`). Restrictive permissions block all pub/sub channels by default, unless explicitly permitted by an ACL rule.

If you use ACLs and pub/sub channels, you should review your databases and ACL settings and plan to transition your cluster to restrictive pub/sub permissions in preparation for future Redis Software releases.

### Prepare for restrictive pub/sub permissions

To secure pub/sub channels and prepare your cluster for future Redis Software releases that default to restrictive pub/sub permissions:

Upgrade Redis databases:

For Redis Software version 6.4.2, upgrade all databases in the cluster to Redis DB version 6.2.

For Redis Software version 7.2, upgrade all databases in the cluster to Redis DB version 7.2 or 6.2.

Create or update ACLs with permissions for specific channels using the `resetchannels &channel` format.

Associate the ACLs with relevant databases.

Set default pub/sub permissions (`acl-pubsub-default`) to restrictive. See [Change default pub/sub permissions](#change-default-pubsub-permissions) for details.

If any issues occur, you can temporarily change the default pub/sub setting back to permissive. Resolve any problematic ACLs before making pub/sub permissions restrictive again.

Note:

When you change the cluster's default pub/sub permissions to restrictive, `&*` is added to the Full Access ACL. Before you make this change, consider the following:

Because pub/sub ACL syntax was added in Redis 6.2, you can't associate the Full Access ACL with database versions 6.0 or lower after this change.

The Full Access ACL is not reverted if you change `acl-pubsub-default` to permissive again.

Every database with the default user enabled uses the Full Access ACL.

### Change default pub/sub permissions

As of Redis Software version 6.4.2, you can configure `acl_pubsub_default`, which determines the default pub/sub permissions for all databases in the cluster. You can set `acl_pubsub_default` to the following values:

`resetchannels` is restrictive and blocks access to all channels by default.

`allchannels` is permissive and allows access to all channels by default.

To make default pub/sub permissions restrictive:

[[operate/rs/installing-upgrading/upgrading/upgrade-database|Upgrade all databases]] in the cluster to Redis version 6.2 or later.

Set the default to restrictive (`resetchannels`) using one of the following methods:

        Cluster Manager UI
      

        rladmin
      

        REST API
      

To set the default pub/sub permissions using the Cluster Manager UI:

Navigate to Access Control > Roles > Pub/Sub ACLs and click Edit.

Read the warning, then click Continue.

For Default permissions for Pub/Sub ACLs, select Restrictive, then Save.

To set the default pub/sub permissions using `rladmin`, run the [`rladmin tune cluster`](/docs/latest/operate/rs/references/cli-utilities/rladmin/tune/#tune-cluster) command:

```sh
rladmin tune cluster acl_pubsub_default resetchannels
```

To set the default pub/sub permissions using the REST API, use an [update cluster policy](/docs/latest/operate/rs/references/rest-api/requests/cluster/policy/#put-cluster-policy) request:

```sh
PUT /v1/cluster/policy
{ "acl_pubsub_default": "resetchannels" }
```

## ACL command support

Redis Software does not support certain Redis ACL commands. Instead, you can manage access controls from the Cluster Manager UI.

Command
Supported

[ACL CAT](/docs/latest/commands/acl-cat/)
â Supported

[ACL DELUSER](/docs/latest/commands/acl-deluser/)
â Not supported

[ACL DRYRUN](/docs/latest/commands/acl-dryrun/)
â Supported

[ACL GENPASS](/docs/latest/commands/acl-genpass/)
â Not supported

[ACL GETUSER](/docs/latest/commands/acl-getuser/)
â Supported

[ACL HELP](/docs/latest/commands/acl-help/)
â Supported

[ACL LIST](/docs/latest/commands/acl-list/)
â Supported

[ACL LOAD](/docs/latest/commands/acl-load/)
â Not supported

[ACL LOG](/docs/latest/commands/acl-log/)
â Not supported

[ACL SAVE](/docs/latest/commands/acl-save/)
â Not supported

[ACL SETUSER](/docs/latest/commands/acl-setuser/)
â Not supported

[ACL USERS](/docs/latest/commands/acl-users/)
â Supported

[ACL WHOAMI](/docs/latest/commands/acl-whoami/)
â Supported

Redis ACLs also have the following differences in Redis Software:

The `MULTI`, `EXEC`, `DISCARD` commands are always allowed, but ACLs are enforced on `MULTI` subcommands.

Nested selectors are not supported.

For example, the following selectors are not valid in Redis Software: `+GET ~key1 (+SET (+SET ~key2) ~key3)`

Key and pub/sub patterns do not allow the following characters: `'(', ')'`

The following password configuration syntax is not supported: `'>', '<', '#!', 'resetpass'`

To configure passwords in Redis Software, use one of the following methods:

[`rladmin cluster reset_password`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/reset_password/):

```sh
rladmin cluster reset_password <user email>
```

REST API [`PUT /v1/users`](/docs/latest/operate/rs/references/rest-api/requests/users/#put-user) request and provide `password`

## See also

[[operate/oss_and_stack/management/security/acl|Redis ACL rules]]

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page