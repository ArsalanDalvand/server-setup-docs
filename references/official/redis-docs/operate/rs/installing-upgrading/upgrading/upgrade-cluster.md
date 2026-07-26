---
title: "Upgrade a Redis Software cluster"
source: "https://redis.io/docs/latest/operate/rs/installing-upgrading/upgrading/upgrade-cluster/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/installing-upgrading/upgrading/upgrade-cluster"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Upgrade a Redis Software cluster"
---
# Upgrade a Redis Software cluster

Upgrade a cluster to a later version of Redis Software.

            Redis Software
          

Before you upgrade a cluster to a later Redis Software version, review the [supported upgrade paths](#supported-upgrade-paths) and [prerequisites](#upgrade-prerequisites).

To upgrade a cluster's Redis Software version, use one of the following methods:

[In-place upgrade](#in-place-upgrade) - Directly upgrade Redis Software on each node in the cluster. Although this method is simpler than the rolling upgrade method, it might cause brief service interruptions as each node is upgraded.

[Rolling upgrade](#rolling-upgrade) - Minimize downtime by adding new nodes with an updated Redis Software version to the cluster, one at a time, while keeping the rest of the cluster operational. This method is recommended for production environments that require continuous availability.

## Supported upgrade paths

â Supported â You can upgrade directly from the current Redis Software cluster version.

❌ Not supported â You cannot upgrade directly from the current Redis Software cluster version. You must first upgrade to a supported intermediate version.

Current Redis Software cluster version
Upgrade to Redis Software 7.2.x
Upgrade to Redis Software 7.4.x
Upgrade to Redis Software 7.8.x
Upgrade to Redis Software 7.22.x
Upgrade to Redis Software 8.0.2-8.0.10
Upgrade to Redis Software 8.0.16-8.0.20
Upgrade to Redis Software 8.2.x

6.0.x
â
❌
❌
❌
❌
❌
❌

6.2.4
6.2.8
â
â
❌
❌
❌
❌
❌

6.2.10
6.2.12
6.2.18
â
â
â
❌
❌
❌
❌

6.4.x
â
â
â
â
â
❌
❌

7.2.x
â
â
â
â
â
â
â

7.4.x
â
â
â
â
â
â
â

7.8.x
â
â
â
â
â
â
â

7.22.x
â
â
â
â
â
â
â

8.0.2
8.0.6
8.0.10
â
â
â
â
â
â
â

8.0.16
8.0.18
8.0.20
â
â
â
â
-
â
â

8.2.x
â
â
â
â
â
â
â

See the [[operate/rs/installing-upgrading/product-lifecycle|Redis Software product lifecycle]] for more information about release numbers and the end-of-life schedule.

Note:
  Redis Enterprise for Kubernetes has its own support lifecycle, which accounts for the Kubernetes distribution lifecycle. For details, see [Supported Kubernetes distributions](/docs/latest/operate/kubernetes/reference/supported_k8s_distributions/).

## Upgrade prerequisites

Before upgrading a cluster:

Verify access to [rlcheck](/docs/latest/operate/rs/references/cli-utilities/rlcheck/) and [rladmin](/docs/latest/operate/rs/references/cli-utilities/rladmin/#use-the-rladmin-shell) commands.

Run [rlcheck](/docs/latest/operate/rs/references/cli-utilities/rlcheck/) on each node and verify there are no issues:

```sh
rlcheck
```

Verify [maintenance mode](/docs/latest/operate/rs/clusters/maintenance-mode/) is not enabled:

On a node in the cluster, run [`rladmin status`](/docs/latest/operate/rs/references/cli-utilities/rladmin/status/):

```sh
rladmin status
```

Review each node's `SHARDS` field. If the value is yellow, the node is in maintenance mode.

[](/docs/latest/images/rs/maintenance_mode.png)

To deactivate maintenance mode on a node, run the following [`rladmin node maintenance_mode off`](/docs/latest/operate/rs/references/cli-utilities/rladmin/node/maintenance-mode/#node-maintenance_mode-off) command. See [Deactivate maintenance mode](/docs/latest/operate/rs/clusters/maintenance-mode/#deactivate-maintenance-mode) for additional details.

```sh
rladmin node <node_id> maintenance_mode off
```

Verify that you meet the upgrade path requirements for the target cluster version and review the relevant [release notes](/docs/latest/operate/rs/release-notes/) for any preparation instructions.

Before you upgrade a cluster from Redis Software version 6.2.x to 7.8.x, you must follow these steps if the cluster has any databases with Redis version 6.0:

Set the Redis upgrade policy to `latest`:

```sh
rladmin tune cluster redis_upgrade_policy latest
```

[[operate/rs/installing-upgrading/upgrading/upgrade-database|Upgrade Redis 6.0 databases]] to Redis 6.2.

[[operate/rs/installing-upgrading/upgrading/upgrade-database|Upgrade your databases]] to a version that is supported by the target Redis Software version before upgrading the cluster. We recommend you upgrade the databases to the latest supported version if possible. Make sure to test the upgrade in a non-production environment to determine any impact.

Avoid changing the database configuration or performing other cluster management operations during the cluster upgrade process, as this might cause unexpected results.

Upgrade the cluster's primary (master) node first. To identify the primary node, use one of the following methods:

Nodes screen in the new Cluster Manager UI (only available for Redis Software versions 7.2 and later)

[`rladmin status nodes`](/docs/latest/operate/rs/references/cli-utilities/rladmin/status/#status-nodes) command

[`GET /nodes/status`](/docs/latest/operate/rs/references/rest-api/requests/nodes/status/#get-all-nodes-status) REST API request

## In-place upgrade

Starting with the primary node, follow these steps for every node in the cluster. To ensure cluster availability, upgrade each node separately.

Complete all [prerequisites](#upgrade-prerequisites) before starting the upgrade.

Verify node operation with the following commands:

```shell
$ rlcheck
$ rladmin status extra all
```

Warning:
  Do not proceed if any shard, node, or endpoint is not `OK`.

Download the Redis Software installation package to the machine running the node from the Download Center on [https://cloud.redis.io](https://cloud.redis.io).

Extract the installation package:

```sh
tar vxf <tarfile name>
```

Note:
  You cannot change the installation path or the user during the upgrade.

Run the install command. See [installation script options](/docs/latest/operate/rs/installing-upgrading/install/install-script/) for a list of command-line options you can add to the following command. You cannot use options marked as "new installs only" during an in-place upgrade.

```shell
sudo ./install.sh
```

The installation script automatically recognizes the upgrade and responds accordingly.

The upgrade replaces all node processes, which might briefly interrupt any active connections.

Verify the node was upgraded to the new version and is still operational:

```shell
$ rlcheck
$ rladmin status extra all
```

Visit the Cluster Manager UI.

If the Cluster Manager UI was open in a web browser during the upgrade, refresh the browser to reload the console.

## Rolling upgrade

To perform a rolling upgrade of the cluster, use one of the following methods:

[Extra node method](#extra-node-upgrade) - recommended if you have additional resources available

[Replace node method](#replace-node-upgrade) - recommended if you cannot temporarily allocate additional resources

### Extra node upgrade method

Complete all [prerequisites](#upgrade-prerequisites) before starting the rolling upgrade.

[[operate/rs/installing-upgrading/install/install-on-linux|Install a later version of Redis Software]] on a new node.

[Add the new node](/docs/latest/operate/rs/clusters/add-node/) to the cluster.

If the [[operate/rs/networking/cluster-dns|cluster uses DNS]], add the new nodeâs IP address to the DNS records.

[[operate/rs/clusters/change-node-role#promote-secondary-node|Promote the first new node]] to become the primary node.

[Remove one node](/docs/latest/operate/rs/clusters/remove-node/#remove-a-node) running the earlier Redis Software version from the cluster.

Repeat the previous steps until all nodes with the earlier Redis Software version are removed. If the final node to remove from the cluster is the primary node, [[operate/rs/clusters/change-node-role#demote-primary-node|demote it]] to a secondary node before you remove it.

### Replace node upgrade method

Complete all [prerequisites](#upgrade-prerequisites) before starting the rolling upgrade.

[Remove a node](/docs/latest/operate/rs/clusters/remove-node/#remove-a-node) with the earlier Redis Software version from the cluster.

Uninstall Redis Software from the removed node:

```sh
sudo ./rl_uninstall.sh
```

[[operate/rs/installing-upgrading/install/install-on-linux|Install a later version of Redis Software]] on the removed node or a new node.

[Add the new node](/docs/latest/operate/rs/clusters/add-node/) to the cluster.

If you want to reuse the removed node's ID when you add the node to the cluster, run [`rladmin cluster join`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/join/) with the `replace_node` flag:

```sh
rladmin cluster join nodes <cluster_member_ip_address> username <username> password <password> replace_node <node_id>
```

If the [[operate/rs/networking/cluster-dns|cluster uses DNS]], add the new nodeâs IP address to the DNS records.

[[operate/rs/clusters/change-node-role#promote-secondary-node|Promote the first new node]] to become the primary node.

Verify node health:

Run `rlcheck` on all nodes:

```sh
rlcheck
```

The output lists the result of each verification test:

```sh
##### Welcome to Redis Software Cluster settings verification utility ####
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

Repeat the previous steps until all nodes with the earlier Redis Software version are replaced. If the final node to remove from the cluster is the primary node, [[operate/rs/clusters/change-node-role#demote-primary-node|demote it]] to a secondary node before you remove it.

## After cluster upgrade

After all nodes are upgraded, the cluster is fully upgraded. Certain features introduced in the new version of Redis Software only become available after upgrading the entire cluster.

After upgrading from version 6.0.x to 6.2.x, restart `cnm_exec` on each cluster node to enable more advanced state machine handling capabilities:

```sh
supervisorctl restart cnm_exec
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page