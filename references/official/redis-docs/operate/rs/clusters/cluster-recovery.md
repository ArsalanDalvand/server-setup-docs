---
title: "Recover a failed cluster"
source: "https://redis.io/docs/latest/operate/rs/clusters/cluster-recovery/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/clusters/cluster-recovery"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Recover a failed cluster"
---
# Recover a failed cluster

How to use the cluster configuration file and database data to recover a failed cluster.

            Redis Software
          

When a Redis Software cluster fails,
you must use the cluster configuration file and database data to recover the cluster.

Note:
  For cluster recovery in a Kubernetes deployment, see [Recover a Redis Enterprise cluster on Kubernetes](/docs/latest/operate/kubernetes/re-clusters/cluster-recovery/).

Cluster failure can be caused by:

A hardware or software failure that causes the cluster to be unresponsive to client requests or administrative actions.
More than half of the cluster nodes lose connection with the cluster, resulting in quorum loss.

To recover a cluster and re-create it as it was before the failure,
you must restore the cluster configuration `ccs-redis.rdb` to the cluster nodes.
To recover databases in the new cluster, you must restore the databases from persistence files such as backup files, append-only files (AOF), or RDB snapshots.
These files are stored in the [[operate/rs/installing-upgrading/install/plan-deployment/persistent-ephemeral-storage|persistent storage location]].

The cluster recovery process includes:

Install Redis Software on the nodes of the new cluster.
Mount the persistent storage with the recovery files from the original cluster to the nodes of the new cluster.
Recover the cluster configuration on the first node in the new cluster.
Join the remaining nodes to the new cluster.
[Recover the databases](/docs/latest/operate/rs/databases/recover/).

## Prerequisites

We recommend that you recover the cluster to clean nodes.
If you use the original nodes,
make sure there are no Redis processes running on any nodes in the new cluster.
We recommend that you use clean persistent storage drives for the new cluster.
If you use the original storage drives,
make sure you back up the files on the original storage drives to a safe location.
Identify the cluster configuration file that you want to use as the configuration for the recovered cluster.
The cluster configuration file is `/ccs/ccs-redis.rdb` on the persistent storage for each node.

## Recover the cluster

(Optional) If you want to recover the cluster to the original cluster nodes, uninstall Redis Software from the nodes.

[[operate/rs/installing-upgrading/install/install-on-linux|Install Redis Software]] on the new cluster nodes.

The new servers must have the same basic hardware and software configuration as the original servers, including:

The same number of nodes
At least the same amount of memory
The same Redis Software version
The same installation user and paths

Note:
  The cluster recovery can fail if these requirements are not met.

Mount the persistent storage drives with the recovery files to the new nodes.
These drives must contain the cluster configuration backup files and database persistence files.

Note:
  Make sure that the user redislabs has permissions to access the storage location
of the configuration and persistence files on each of the nodes.

If you use local persistent storage, place all of the recovery files on each of the cluster nodes.

To recover the original cluster configuration, run [`rladmin cluster recover`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/recover/) on the first node in the new cluster:

```sh
rladmin cluster recover filename [ <persistent_path> | <ephemeral_path> ]<filename> node_uid <node_uid> rack_id <rack_id>
```

For example:

```sh
rladmin cluster recover filename /tmp/persist/ccs/ccs-redis.rdb node_uid 1 rack_id 5
```

When the recovery command succeeds,
this node is configured as the node from the old cluster that has ID 1.

To join the remaining servers to the new cluster, run [`rladmin cluster join`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/join/) from each new node:

```sh
rladmin cluster join nodes <cluster_member_ip_address> username <username> password <password> replace_node <node_id>
```

For example:

```sh
rladmin cluster join nodes 10.142.0.4 username [email protected] password mysecret replace_node 2
```

Run [`rladmin status`](/docs/latest/operate/rs/references/cli-utilities/rladmin/status/) to verify the recovered nodes are now active and the databases are pending recovery:

```sh
rladmin status
```

Note:
  Make sure that you update your [[operate/rs/networking/cluster-dns|DNS records]]
with the IP addresses of the new nodes.

After the cluster is recovered, you must [recover the databases](/docs/latest/operate/rs/databases/recover/).

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page