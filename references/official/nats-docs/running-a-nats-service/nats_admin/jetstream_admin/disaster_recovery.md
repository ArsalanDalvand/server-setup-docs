---
title: "Disaster Recovery"
source: "https://docs.nats.io/running-a-nats-service/nats_admin/jetstream_admin/disaster_recovery.md"
source_format: official-markdown
language: "en"
doc_slug: "running-a-nats-service/nats_admin/jetstream_admin/disaster_recovery"
category: "running-a-nats-service"
retrieved_at: "2026-07-28T21:36:55+00:00"
tags:
  - nats
  - pubsub-messaging
  - jetstream
  - official-documentation
aliases:
  - "Disaster Recovery"
---
# Disaster Recovery

In the event of unrecoverable JetStream message persistence on one (or more) server nodes, there are two recovery scenarios:

* Automatic recovery from intact quorum nodes
* Manual recovery from existing stream snapshots (backups)

{% hint style="danger" %}
For R1 streams, data is persisted on one server node only. If that server node is unrecoverable then recovery from backup is the sole option.
{% endhint %}

## Automatic Recovery

NATS will create replacement stream replicas automatically under the following conditions:

* Impacted stream is of replica configuration R3 (or greater)
* Remaining intact nodes (stream replicas) meet minimum RAFT quorum: floor(R/2) + 1
* Available node(s) in the stream's cluster for new replica(s)
* Impacted node(s) removed from the stream's domain RAFT Meta group (e.g. `nats server cluster peer-remove`)

## Manual Recovery

Snapshots (also known as backups) can pro-actively be made of any stream regardless of replication configuration.

The backup includes (by default):

* Stream configuration and state
* Stream durable consumer configuration and state
* All message payload data including metadata like timestamps and headers

### Backup

The `nats stream backup` CLI command is used to create snapshots of a stream and its durable consumers.

{% hint style="info" %}
As an account owner, if you wish to make a backup of ALL streams in your account, you may use `nats account backup` instead.
{% endhint %}

{% hint style="warning" %}
Memory storage streams do not support snapshots. Only file-based storage streams can be backed up.
{% endhint %}

```shell
nats stream backup ORDERS '/data/js-backup/backup1'
```

Output

```
Starting backup of Stream "ORDERS" with 13 data blocks

2.4 MiB/s [====================================================================] 100%

Received 13 MiB bytes of compressed data in 3368 chunks for stream "ORDERS" in 1.223428188s, 813 MiB uncompressed
```

During a backup operation, the stream is placed in a status where it's configuration cannot change and no data will be evicted based on stream retention policies.

{% hint style="info" %}
Progress using the terminal bar can be disabled using `--no-progress`, it will then issue log lines instead.
{% endhint %}

### Restore

An existing backup (as above) can be restored to the same or a new NATS server (or cluster) using the `nats stream restore` command.

{% hint style="info" %}
`nats stream restore` restores a single stream from one backup directory. To restore all streams at once, use `nats account restore` as described below.
{% endhint %}

```shell
nats stream restore '/data/js-backup/backup1'
```

Output

```
Starting restore of Stream "ORDERS" from file "/data/js-backup/backup1"

13 MiB/s [====================================================================] 100%

Restored stream "ORDERS" in 937.071149ms

Information for Stream ORDERS

Configuration:

             Subjects: ORDERS.>
...
```

Progress using the terminal bar can be disabled using `--no-progress`, it will then issue log lines instead.

## Account-Level Backup and Restore

In environments where the `nats` CLI is used interactively to configure the server you do not have a desired state to recreate the server from. This is not the ideal way to administer the server, we recommend Configuration Management, but many will use this approach.

The `nats account backup` and `nats account restore` commands allow you to back up and restore all streams in an account at once, including their configuration, consumer state, and all message data.

### Account Backup

```shell
nats account backup /data/js-backup
```

Output

```
Performing backup of all streams to /data/js-backup

    Streams: 3
       Size: 14 KiB
  Consumers: 2

Starting backup of Stream "EVENTS" with 0 B
Received 1.5 KiB compressed data in 2 chunks for stream "EVENTS" in 0s, 16 KiB uncompressed

Starting backup of Stream "ORDERS" with 55 B
Received 976 B compressed data in 2 chunks for stream "ORDERS" in 1ms, 9.5 KiB uncompressed

Starting backup of Stream "WORK" with 7.3 KiB
Received 7.3 KiB compressed data in 2 chunks for stream "WORK" in 0s, 30 KiB uncompressed
```

This creates a subdirectory per stream inside `/data/js-backup`, each containing the full stream snapshot (configuration, consumer state, and message data) in the same format as `nats stream backup`.

Available flags for `nats account backup`:

| Flag                  | Description                                  |
| --------------------- | -------------------------------------------- |
| `--consumers`         | Include consumer configuration and state     |
| `--check`             | Check backup integrity                       |
| `--force`             | Force overwrite of existing backup directory |
| `--critical-warnings` | Treat warnings as critical errors            |

### Account Restore

```shell
nats account restore /data/js-backup
```

Output

```
Restoring backup of all 3 streams in directory "/data/js-backup"

Starting restore of Stream "EVENTS" from file "/data/js-backup/EVENTS"
Restored stream "EVENTS" in 0s
...

Starting restore of Stream "ORDERS" from file "/data/js-backup/ORDERS"
Restored stream "ORDERS" in 1ms
...

Starting restore of Stream "WORK" from file "/data/js-backup/WORK"
Restored stream "WORK" in 0s
...
```

This restores all stream subdirectories found in `/data/js-backup`, including their full message data and consumer state.

Available flags for `nats account restore`:

| Flag        | Description                         |
| ----------- | ----------------------------------- |
| `--cluster` | Target cluster for restored streams |
| `--tag`     | Placement tag for restored streams  |

{% hint style="warning" %}
`nats account restore` will fail if a stream with the same name already exists. You must remove the existing stream before restoring from backup.
{% endhint %}


---

# Agent Instructions
This documentation is published with GitBook. GitBook is the documentation platform designed so that both humans and AI agents can read, navigate, and reason over technical content effectively. Learn more at gitbook.com.

## Querying This Documentation
If you need additional information that is not directly available in this page, you can query the documentation dynamically by asking a question.

Perform an HTTP GET request on the current page URL with the `ask` query parameter, and the optional `goal` query parameter:

```
GET https://docs.nats.io/running-a-nats-service/nats_admin/jetstream_admin/disaster_recovery.md?ask=<question>&goal=<endgoal>
```

`ask` is the immediate question: it should be specific, self-contained, and written in natural language.
`goal` is optional and describes the broader end goal you are ultimately trying to accomplish on behalf of the user. GitBook uses it to tailor the answer towards what is most useful for that goal.

The response will contain a direct answer to the question and relevant excerpts and sources from the documentation.

Use this mechanism when the answer is not explicitly present in the current page, you need clarification or additional context, or you want to retrieve related documentation sections.
