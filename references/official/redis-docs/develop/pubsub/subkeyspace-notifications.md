---
title: "Redis subkey notifications"
source: "https://redis.io/docs/latest/develop/pubsub/subkeyspace-notifications/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "develop/pubsub/subkeyspace-notifications"
category: "develop"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Redis subkey notifications"
---
# Redis subkey notifications

Monitor changes to individual subkeys in real time

Subkey notifications, added in Redis 8.8, extend Redis's existing [[develop/pubsub/keyspace-notifications|keyspace notification]] system to include the key, the subkey (for example, the field for hashes, the path for JSON documents, and the element for arrays), and the event type.

With standard keyspace notifications, when a hash field is modified via [`HSET`](/docs/latest/commands/hset/), [`HDEL`](/docs/latest/commands/hdel/), or [`HEXPIRE`](/docs/latest/commands/hexpire/), the subscriber receives the key name and the event type but not which specific fields were affected. Subkey notifications solve this by carrying the affected field names in the message payload.

Subkey notifications are delivered through Pub/Sub channels and are independent of the standard keyspace/keyevent notification channels. Enabling subkey notifications does not implicitly enable standard keyspace notifications, and vice versa.

Note: Redis Pub/Sub is fire and forget. If your Pub/Sub client disconnects and reconnects later, all events delivered during the disconnection period are lost.

### Notification channels

Four new channel types are available, each suited to a different subscription pattern:

Channel format
Payload

`__subkeyspace@<db>__:<key>`
`<event>|<len>:<subkey>[,...]`

`__subkeyevent@<db>__:<event>`
`<key_len>:<key>|<len>:<subkey>[,...]`

`__subkeyspaceitem@<db>__:<key>\n<subkey>`
`<event>`

`__subkeyspaceevent@<db>__:<event>|<key>`
`<len>:<subkey>[,...]`

Design rationale:

Subkeyspace (`__subkeyspace@<db>__:<key>`): Subscribe to a specific key; each message contains an event type and subkey names.
Subkeyevent (`__subkeyevent@<db>__:<event>`): Subscribe to a specific event type; each message contains a key name and subkey names.
Subkeyspaceitem (`__subkeyspaceitem@<db>__:<key>\n<subkey>`): Subscribe to a specific key and subkey name combination; each message contains an event type.
Subkeyspaceevent (`__subkeyspaceevent@<db>__:<event>|<key>`): Subscribe to a specific event and key combination; each message contains subkey names.

Subkeys in the payload are encoded in a length-prefixed format (`<len>:<subkey>`) to support binary-safe subkey names that may contain delimiter characters.

Safeguards:

Events whose name contains `|` are skipped for the `__subkeyspace` and `__subkeyspaceevent` channels to avoid parsing ambiguity.
Keys containing `\n` are skipped for the `__subkeyspaceitem` channel because newline is the key/subkey separator.
Subkey events are only published when at least one subkey is present.

### Configuration

Subkey notifications are controlled by the existing `notify-keyspace-events` configuration string. Four new flag characters are added:

```
S     Subkeyspace events, published with __subkeyspace@<db>__ prefix.
T     Subkeyevent events, published with __subkeyevent@<db>__ prefix.
I     Subkeyspaceitem events, published with __subkeyspaceitem@<db>__ prefix.
V     Subkeyspaceevent events, published with __subkeyspaceevent@<db>__ prefix.
h     Hash commands.
```

These flags are independent from the existing key-level flags (`K`, `E`, and so on). You may enable any combination. For example, to enable only the subkeyspace and subkeyevent channels:

```
$ redis-cli config set notify-keyspace-events ST
```

To enable all four subkey channel types:

```
$ redis-cli config set notify-keyspace-events STIV
```

### Commands emitting subkey notifications

The following commands emit subkey notifications. Currently, only hash commands are supported; support for additional data types is planned for future releases.

Command
Event
Subkeys included

[`HSET`](/docs/latest/commands/hset/) / [`HMSET`](/docs/latest/commands/hmset/)
`hset`
All fields being set

[`HSETNX`](/docs/latest/commands/hsetnx/)
`hset`
The field (only if it was set)

[`HDEL`](/docs/latest/commands/hdel/)
`hdel`
All fields deleted

[`HGETDEL`](/docs/latest/commands/hgetdel/)
`hdel` / `hexpired`
Deleted or lazily expired fields

[`HGETEX`](/docs/latest/commands/hgetex/)
`hexpire` / `hpersist` / `hdel` / `hexpired`
Affected fields per event type

[`HINCRBY`](/docs/latest/commands/hincrby/)
`hincrby`
The field

[`HINCRBYFLOAT`](/docs/latest/commands/hincrbyfloat/)
`hincrbyfloat`
The field

[`HEXPIRE`](/docs/latest/commands/hexpire/) / [`HPEXPIRE`](/docs/latest/commands/hpexpire/) / [`HEXPIREAT`](/docs/latest/commands/hexpireat/) / [`HPEXPIREAT`](/docs/latest/commands/hpexpireat/)
`hexpire`
Fields whose TTLs were updated

[`HPERSIST`](/docs/latest/commands/hpersist/)
`hpersist`
Fields that were persisted

[`HSETEX`](/docs/latest/commands/hsetex/)
`hset` / `hdel` / `hexpire` / `hexpired`
Affected fields per event type

Subkey expiration (active or lazy)
`hexpired`
All expired fields, batched into a single notification

### Watching events in real time

To observe subkey notifications, enable the desired channel types and use `redis-cli` to subscribe with a glob pattern:

```
$ redis-cli config set notify-keyspace-events ST
$ redis-cli --csv psubscribe '__subkey*'
Reading messages... (press Ctrl-C to quit)
"psubscribe","__subkey*",1
```

Then, in another terminal, run a command:

```
$ redis-cli hset myhash field1 val1 field2 val2
```

You will see messages similar to the following:

```
"pmessage","__subkey*","__subkeyspace@0__:myhash","hset|7:field1,7:field2"
"pmessage","__subkey*","__subkeyevent@0__:hset","5:myhash|7:field1,7:field2"
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page