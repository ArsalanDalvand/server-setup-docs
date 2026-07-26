---
title: "Roles"
source: "https://stalw.art/docs/cluster/configuration/roles"
source_format: official-html-to-markdown
language: "en"
doc_slug: "cluster/configuration/roles"
category: "cluster"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Roles"
---
# Roles

      

In a clustered deployment, background and maintenance tasks must run regularly to keep the system healthy and secure. To distribute this operational workload across the cluster, Stalwart models each role as a named profile applied to one or more nodes.

Roles are defined through [[ref/object/cluster-role|ClusterRole]] objects (found in the WebUI under  Settings ›  Cluster › Roles). Each role selects a set of tasks and network listeners, and nodes that start up with a given role name run only the tasks and listeners enabled for that role. This distributes work evenly and lets administrators keep resource-intensive tasks on dedicated hardware.

Each ClusterRole carries the following fields:

[[ref/object/cluster-role#name|`name`]]: a unique identifier for the role (read-only).
[[ref/object/cluster-role#description|`description`]]: a human-readable description.
[[ref/object/cluster-role#tasks|`tasks`]]: a [[ref/object/cluster-role#clustertaskgroup|ClusterTaskGroup]] selecting which cluster tasks the role enables. The variants are `EnableAll`, `DisableAll`, `EnableSome`, and `DisableSome`; the `EnableSome` and `DisableSome` variants carry a list of task types via `taskTypes`.
[[ref/object/cluster-role#listeners|`listeners`]]: a [[ref/object/cluster-role#clusterlistenergroup|ClusterListenerGroup]] selecting which network listeners the role enables. The variants mirror those of `tasks`, with a `listenerIds` field referencing [[ref/object/network-listener|NetworkListener]] ids.

A node is assigned to a role through the [[configuration/environment-variables#clustering|`STALWART_ROLE`]] environment variable, which names the ClusterRole whose tasks and listeners the node should run.

## Available tasks

[Section titled “Available tasks”](#available-tasks)

The tasks that a role can enable or disable are drawn from the `ClusterTaskType` enumeration on the [[ref/object/cluster-role#clustertasktype|ClusterRole]] object:

`storeMaintenance`: purges expired data from the data store, keeping it compact and performant.
`accountMaintenance`: cleans up deleted accounts, old changelog entries, and related records.
`metricsCalculate`: calculates and aggregates metrics and statistics for the cluster.
`metricsPush`: exports metrics to external monitoring or visualisation systems.
`pushNotifications`: delivers push notifications to subscribed clients.
`searchIndexing`: performs full-text search indexing on new or modified content.
`spamClassifierTraining`: runs spam classifier training. Only one node should run this task at a time to avoid conflicting model updates.
`outboundMta`: processes outbound mail through the MTA queue.
`taskQueueProcessing`: executes queued background tasks.
`taskScheduler`: schedules recurring and deferred tasks. ACME renewal, iMIP processing, calendar-alert dispatch, and similar time-driven jobs are dispatched by the task scheduler.

Task assignment is driven entirely by the [[ref/object/cluster-role|ClusterRole]] object and the `STALWART_ROLE` environment variable. The role named in `STALWART_ROLE` selects both which [[ref/object/cluster-role#tasks|`tasks`]] the node runs and which [[ref/object/cluster-role#listeners|`listeners`]] it exposes, so a single role definition captures the full workload of a given node.

## Push notification sharding

[Section titled “Push notification sharding”](#push-notification-sharding)

Push notification delivery must be sharded so that each subscriber receives exactly one notification across the cluster. A node is placed in a specific shard by setting the [[configuration/environment-variables#clustering|`STALWART_PUSH_SHARD`]] environment variable. Nodes in the same shard cooperate to deliver the subset of notifications assigned to that shard. Sharding is configured exclusively through this environment variable; ClusterRole does not carry shard groupings.

## Example

[Section titled “Example”](#example)

A simple two-role deployment might define a `frontend` role that serves client-facing listeners but runs no background tasks, and a `maintenance` role that runs every background task but exposes no client listeners:

```
{  "name": "frontend",  "description": "Serves IMAP, JMAP, SMTP and WebDAV traffic",  "tasks": {    "@type": "DisableAll"  },  "listeners": {    "@type": "EnableAll"  }}
```

```
{  "name": "maintenance",  "description": "Runs store maintenance, indexing, and metrics",  "tasks": {    "@type": "EnableSome",    "taskTypes": [      "storeMaintenance",      "accountMaintenance",      "searchIndexing",      "metricsCalculate",      "metricsPush"    ]  },  "listeners": {    "@type": "DisableAll"  }}
```

Nodes that start with `STALWART_ROLE=frontend` run only the network listeners, while nodes started with `STALWART_ROLE=maintenance` run only the selected background tasks.

## Best practices

[Section titled “Best practices”](#best-practices)

Role planning should start from the resource profile of each task. Tasks such as `searchIndexing` and `spamClassifierTraining` tend to be CPU- and I/O-intensive; `metricsCalculate` and scheduled maintenance are lighter but time-sensitive. Assigning resource-heavy roles to nodes with sufficient computational capacity and isolating them from latency-sensitive workloads helps keep the cluster responsive.

In high-availability deployments, critical tasks should run on multiple nodes so that the failure of one does not stop routine maintenance. Where duplicate work is a concern, for example push notifications, sharding through `STALWART_PUSH_SHARD` prevents the same work from being done twice.

When sharding, aim for an even distribution of nodes across shards. Each shard should have similar computational resources to prevent uneven load. In smaller deployments a single shard may be sufficient.

Role assignments should be reviewed as the cluster evolves. Changes in user population, message volume, or data growth may shift the optimal distribution of responsibilities. Regular audits of node health, performance metrics, and quota utilisation help identify when roles should be reassigned, split, or consolidated.