---
title: "Change node roles"
source: "https://redis.io/docs/latest/operate/rs/clusters/change-node-role/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/clusters/change-node-role"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Change node roles"
---
# Change node roles

Change node roles to demote the primary node to a secondary node or promote a secondary node to become the primary node.

            Redis Software
          

A Redis Software cluster contains a primary node, which coordinates cluster-wide management operations, and multiple secondary nodes. Nodes with either role can host database shards.

## Demote primary node

        Cluster Manager UI
      

        rladmin
      

To demote the primary node to a secondary node using the Cluster Manager UI:

On the Nodes screen, click the More actions button (â®) for the primary node you want to demote.

Select Set as a secondary node from the list.

Select one of the options to determine the new primary node:

Automatically: The cluster decides which node becomes the new primary node.

Choose specific node: You can manually select which node becomes the new primary node.

Click Confirm.

To demote the primary node to a secondary node using `rladmin`:

Identify the primary node's ID with [`rladmin cluster master`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/master/):

```sh
$ rladmin cluster master
Node <primary-node-id> is the cluster master node
```

Run [`rladmin node enslave`](/docs/latest/operate/rs/references/cli-utilities/rladmin/node/enslave/#node-enslave) with the `demote_node` option:

```sh
rladmin node <primary-node-ID> enslave demote_node
```

Replace `<primary-node-ID>` with the ID returned by `rladmin cluster master`.

## Promote secondary node

        Cluster Manager UI
      

        rladmin
      

To promote a secondary node to become the primary node using the Cluster Manager UI:

On the Nodes screen, click the More actions button (â®) for the secondary node you want to promote.

Select Set as the primary node from the list.

Click Confirm.

To promote a secondary node to become the primary node using `rladmin`:

To find the IDs of secondary nodes, run [`rladmin status nodes`](/docs/latest/operate/rs/references/cli-utilities/rladmin/status/#status-nodes):

```sh
$ rladmin status nodes
CLUSTER NODES:
NODE:ID     ROLE       ADDRESS          EXTERNAL_ADDRESS          HOSTNAME            SHARDS     CORES          FREE_RAM                 PROVISIONAL_RAM          VERSION        STATUS   
node:1      master     198.51.100.2                                 3d99db1fdf4b        4/100      6              14.74GB/19.54GB          10.73GB/16.02GB          7.22.0-250     OK       
*node:3     slave      198.51.100.4                                 b87cc06c830f        0/100      6              14.74GB/19.54GB          11.22GB/16.02GB          7.22.0-250     OK       
node:2      slave      198.51.100.3                                 fc7a3d332458        0/100      6              14.74GB/19.54GB          11.22GB/16.02GB          7.22.0-250     OK
```

Nodes with the `slave` role are secondary nodes.

Run [`rladmin cluster master set`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/master/):

```sh
rladmin cluster master set <secondary-node-ID>
```

Replace `<secondary-node-ID>` with the ID of the secondary node you want to promote.

After this node becomes the primary node, all cluster management traffic is directed to it.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page