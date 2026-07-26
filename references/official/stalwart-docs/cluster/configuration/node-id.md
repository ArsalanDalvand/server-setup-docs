---
title: "Node ID"
source: "https://stalw.art/docs/cluster/configuration/node-id"
source_format: official-html-to-markdown
language: "en"
doc_slug: "cluster/configuration/node-id"
category: "cluster"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Node ID"
---
# Node ID

      

Each Stalwart instance in a cluster must have a unique identifier that distinguishes it from other nodes. The identifier drives coordination, logging, and internal message routing.

Node identifiers are assigned automatically through a lease mechanism. When a node starts, it reads its operating-system hostname and uses that value to acquire a node-id lease from the cluster. The node then renews the lease periodically while it remains online. As long as each node in the cluster has a unique hostname, a distinct node id is assigned to each node without any manual configuration.

If two nodes share the same hostname they will compete for the same lease, which causes coordination conflicts and data inconsistencies. Ensuring that every node runs with a unique hostname is therefore a prerequisite for joining a cluster.

When the operating-system hostname cannot be changed or is not unique across the cluster, the value used to acquire the lease can be overridden by setting the [[configuration/environment-variables#clustering|`STALWART_HOSTNAME`]] environment variable before the server starts. The override replaces the system hostname only for the purposes of node identification; the operating system itself is left untouched.

## ClusterNode

[Section titled “ClusterNode”](#clusternode)

The [[ref/object/cluster-node|ClusterNode]] object (found in the WebUI under  Management ›  Cluster) is a read-only view over the cluster’s node registry. Querying it returns one entry per registered node with the following fields:

[[ref/object/cluster-node#nodeid|`nodeId`]]: the integer identifier assigned to the node.
[[ref/object/cluster-node#hostname|`hostname`]]: the operating-system hostname of the node.
[[ref/object/cluster-node#lastrenewal|`lastRenewal`]]: the timestamp of the node’s most recent lease renewal.
[[ref/object/cluster-node#status|`status`]]: the current status of the node (`active`, `stale`, or `inactive`).

A typical entry returned by the registry looks like:

```
{  "nodeId": 1,  "hostname": "mx1.example.org",  "lastRenewal": "2026-01-01T00:00:00Z",  "status": "active"}
```

Because ClusterNode is read-only, node registration cannot be edited directly: nodes appear in the registry as they acquire leases and their status is updated as they renew or fail to renew. Administrators use the registry to inspect cluster membership, confirm that each host has obtained an identifier, and detect stale or inactive nodes.