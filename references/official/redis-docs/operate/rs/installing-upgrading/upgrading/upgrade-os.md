---
title: "Upgrade a cluster's operating system"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/upgrading/upgrade-os/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/upgrading/upgrade-os"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Upgrade a cluster's operating system"
---
# Upgrade a cluster's operating system

Upgrade a Redis Software cluster's operating system to a later major version.

            Redis Software
          

To upgrade the operating system (OS) on a Redis Software cluster to a later major version, perform a rolling upgrade. Because you upgrade one node at a time, you can upgrade your cluster's OS without downtime.

## Prerequisites

Before you upgrade a cluster's operating system:

[[operate/rs/installing-upgrading/upgrading/upgrade-cluster|Upgrade all nodes in the cluster]] to a Redis Software version that supports the OS's current version and upgrade version.

To learn which versions of Redis Software support specific OS versions, see [Supported platforms](/docs/latest/operate/rs/references/supported-platforms/#supported-platforms).

If the cluster uses custom directories, make sure the OS upgrade version also supports custom directories, and specify the same custom directories during installation for all nodes. See [Customize installation directories](/docs/latest/operate/rs/installing-upgrading/install/customize-install-directories/) for details.

## Perform OS rolling upgrade

To upgrade the cluster's operating system, use one of the following rolling upgrade methods:

[Extra node method](#extra-node-upgrade) - recommended if you have additional resources available

[Replace node method](#replace-node-upgrade) - recommended if you cannot temporarily allocate additional resources

### Extra node upgrade method

Complete all [prerequisites](#prerequisites) before starting the rolling upgrade.

Create a node with the OS upgrade version.

[[operate/rs/installing-upgrading/install/install-on-linux|Install the cluster's current Redis Software version]] on the new node using the installation package for the OS upgrade version.

[Add the new node](/docs/latest/operate/rs/clusters/add-node/)  to the cluster.

If the [[operate/rs/networking/cluster-dns|cluster uses DNS]], add the new nodeâs IP address to the DNS records.

[Remove one node](/docs/latest/operate/rs/clusters/remove-node/#remove-a-node) running the earlier OS version from the cluster.

Repeat the previous steps until all nodes with the earlier OS version are removed. If the final node to remove from the cluster is the primary node, [[operate/rs/clusters/change-node-role#demote-primary-node|demote it]] to a secondary node before you remove it.

### Replace node upgrade method

Complete all [prerequisites](#prerequisites) before starting the rolling upgrade.

[Remove a node](/docs/latest/operate/rs/clusters/remove-node/#remove-a-node) with the earlier OS version from the cluster.

Uninstall Redis Software from the removed node:

```sh
sudo ./rl_uninstall.sh
```

Either upgrade the existing node to the OS upgrade version, or create a new node with the OS upgrade version.

[[operate/rs/installing-upgrading/install/install-on-linux|Install the cluster's current Redis Software version]] on the upgraded node using the installation package for the OS upgrade version.

[Add the new node](/docs/latest/operate/rs/clusters/add-node/) to the cluster.

If you want to reuse the removed node's ID when you add the node to the cluster, run [`rladmin cluster join`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/join/) with the `replace_node` flag:

```sh
rladmin cluster join nodes <cluster_member_ip_address> username <username> password <password> replace_node <node_id>
```

If the [[operate/rs/networking/cluster-dns|cluster uses DNS]], add the new nodeâs IP address to the DNS records.

Verify node health:

Run `rlcheck` on all nodes:

```sh
rlcheck
```

The output lists the result of each verification test:

```sh
##### Welcome to Redis Enterprise Cluster settings verification utility ####
Running test: verify_bootstrap_status
                PASS
...
Running test: verify_encrypted_gossip
                PASS
Summary:
-------
ALL TESTS PASSED.
```

For healthy nodes, the expected output is `ALL TESTS PASSED`.

Run [`rladmin status`](/docs/latest/operate/rs/references/cli-utilities/rladmin/status/) on the new node:

```sh
rladmin status extra all
```

The expected output is the `OK` status for the cluster, nodes, endpoints, and shards:

```sh
CLUSTER:
OK. Cluster master: 2 (<IP.address>)
Cluster health: OK, [0, 0.0, 0.0]
failures/minute - avg1 0.00, avg15 0.00, avg60 0.00.
...
```

Repeat the previous steps until all nodes with the earlier OS version are replaced. If the final node to remove from the cluster is the primary node, [[operate/rs/clusters/change-node-role#demote-primary-node|demote it]] to a secondary node before you remove it.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page