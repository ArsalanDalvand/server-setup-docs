---
title: "Rack-zone awareness in Redis Software"
source: "https://redis.io/docs/latest/operate/rs/clusters/configure/rack-zone-awareness/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/clusters/configure/rack-zone-awareness"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Rack-zone awareness in Redis Software"
---
# Rack-zone awareness in Redis Software

Rack-zone awareness ensures high availability in the event of a rack or zone failure.

            Redis Software
          

            Redis Enterprise for Kubernetes
          

Rack-zone awareness helps ensure high availability in the event of a rack or zone failure.

When you enable rack-zone awareness in a Redis Software cluster, you assign
a [rack-zone ID](#rack-zone-id-rules) to each node. This ID is used to map the node to a
physical rack or logical zone. The cluster can then ensure that primary shards, corresponding replica shards, and associated endpoints are placed on [nodes in different racks or zones](#node-layout-guidelines).

In the event of a rack or zone failure, the replicas and endpoints in the remaining racks and zones are promoted. This ensures high availability when a rack or zone fails.

There is no limitation on the number of racks and zones per cluster. Each
node can belong to a different rack or multiple nodes can belong to the
same rack.

Rack-zone awareness affects various cluster, node, and database actions, such as node rebalancing, node removal, node replacement, shard and endpoint migration, and database failover.

## Rack-zone ID rules

The rack-zone ID must comply with the following rules:

Maximum length of 63 characters.
Characters consist of letters, digits, and hyphens ('-'). Underscores ('_') are also accepted as of Redis Software [6.4.2-61](/docs/latest/operate/rs/release-notes/rs-6-4-2-releases/rs-6-4-2-61/).
ID starts with a letter and ends with a letter or a digit.

Note:
  Rack-zone IDs are case-insensitive (uppercase and lowercase letter are treated as the same).

## Node layout guidelines

Avoid placing the majority of nodes in one availability zone.

If a Redis Software cluster consists of three nodes (the recommended minimum), follow these guidelines:

For high availability, the three nodes must be distributed across three distinct racks or zones.

When using availability zones, all three zones should exist within the same region to avoid potential latency issues.

## Set up rack-zone awareness

To enable rack-zone awareness, you need to configure it for the
cluster, nodes, and [databases](#enable-database-rack-zone-awareness).

Note:

After rack-zone awareness is enabled for a cluster, it cannot be turned off.

After a rack ID is assigned to a node, it cannot be changed.

### New cluster

You can set up rack-zone awareness for the cluster and its nodes during [[operate/rs/clusters/new-cluster-setup|cluster creation]]:

In the Cluster screen's Configuration section, enable Rack zone awareness.

Select Next to continue to the Node configuration screen.

Enter a Rack-zone ID for the current node.

Finish [[operate/rs/clusters/new-cluster-setup|cluster setup]].

For every [node you add to the cluster](/docs/latest/operate/rs/clusters/add-node/), assign a different Rack-zone ID.

### Existing cluster

If you did not configure rack-zone awareness during cluster creation, you can configure rack-zone awareness for existing clusters using the [REST API](/docs/latest/operate/rs/references/rest-api/):

For each node in the cluster, assign a different rack-zone ID using the REST API to [update the node](/docs/latest/operate/rs/references/rest-api/requests/nodes/#put-node):

```sh
PUT /v1/nodes/<node-ID>
{ "rack_id": "rack-zone-ID" }
```

[Update the cluster policy](/docs/latest/operate/rs/references/rest-api/requests/cluster/policy/#put-cluster-policy) to enable rack-zone awareness:

```sh
PUT /v1/cluster/policy
{ "rack_aware": true }
```

## Set up two-dimensional rack-zone awareness

As of Redis Software version 7.22, you can assign a `second_rack_id` to set up two-dimensional rack-zone awareness.

You can use two-dimensional rack-zone awareness to create logical zones within a zone or rack.

### New clusters

To set up two-dimensional rack-zone awareness during cluster creation, assign a `second_rack_id` to each node in the cluster in addition to the `rack_id` using the [REST API](/docs/latest/operate/rs/references/rest-api/) or [rladmin](/docs/latest/operate/rs/references/cli-utilities/rladmin/).

#### REST API method

To create a new cluster with two-dimensional rack-zone awareness, you can use [bootstrap REST API requests](/docs/latest/operate/rs/references/rest-api/requests/bootstrap/#post-bootstrap):

Create the new cluster on the first node, set `rack_aware` to `true`, and assign a `rack_id` and `second_rack_id` to the first node:

```sh
POST /v1/bootstrap/create_cluster
{
  "action": "create_cluster",
  "cluster": {
    "nodes": [],
    "name": "<cluster.fqdn>"
  },
  "credentials": {
    "username": "<admin-email>",
    "password": "<admin-password>"
  },
  "node": {
    "identity": {
        "rack_id": "<availability-zone-ID>",
        "second_rack_id": "<rack-ID>"
    }
  },
  "policy": {
    "rack_aware": true
  }
}
```

Join each new node you want to add to the cluster and assign a different `rack_id` and `second_rack_id` to it:

```sh
POST /v1/bootstrap/join_cluster
{
  "action": "join_cluster",
  "cluster": {
     "nodes": [],
     "name": "<cluster.fqdn>"
  },
  "credentials": {
    "username": "<admin-email>",
    "password": "<admin-password>"
  },
  "node": {
    "identity": {
        "rack_id": "<availability-zone-ID>",
        "second_rack_id": "<rack-ID>"
    }
  }
}
```

#### Command-line method

To create a new cluster with two-dimensional rack-zone awareness using the command line:

Run [`rladmin cluster create`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/create/) to create the initial cluster on one node, enable rack-zone awareness, and assign a `rack_id` and `second_rack_id`:

```sh
$ rladmin cluster create name <cluster-name> \
    username <admin-email> \
    password <admin-password> \
    rack_aware \
    rack_id <node-rack-ID> \
    second_rack_id <second-node rack-ID>
```

Run [`rladmin cluster join`](/docs/latest/operate/rs/references/cli-utilities/rladmin/cluster/join/) for each new node you want to add to the cluster and assign a different `rack_id` and `second_rack_id`:

```sh
$ rladmin cluster join nodes <node-IP-address> \
    username <admin-email> \
    password <admin-password> \
    rack_id <node-rack ID> \
    second_rack_id <second-node-rack-ID>
```

### Existing clusters

You can configure two-dimensional rack-zone awareness for existing clusters using the [REST API](/docs/latest/operate/rs/references/rest-api/).

For each node in the cluster, assign a different `second_rack_id` using the REST API to [update the node](/docs/latest/operate/rs/references/rest-api/requests/nodes/#put-node):

```sh
PUT /v1/nodes/<node-ID>
{ "second_rack_id": "rack-ID" }
```

## Enable database rack-zone awareness

Before you can enable rack-zone awareness for a database, you must configure rack-zone awareness for the cluster and its nodes. For more information, see [set up rack-zone awareness](#set-up-rack-zone-awareness).

To enable rack-zone awareness for a database, use a [REST API request](/docs/latest/operate/rs/references/rest-api/requests/bdbs/#put-bdbs):

```sh
PUT /v1/bdbs/<database-ID>
{ "rack_aware": true }
```

### Rearrange database shards

After you enable rack-zone awareness for an existing database, you should generate an optimized shard placement blueprint using the [REST API](/docs/latest/operate/rs/references/rest-api/) and use it to rearrange the shards in different racks or zones.

[Generate an optimized shard placement blueprint](/docs/latest/operate/rs/references/rest-api/requests/bdbs/actions/optimize_shards_placement/#get-bdbs-actions-optimize-shards-placement):

Send the following `GET` request:

```sh
GET /v1/bdbs/<database-ID>/actions/optimize_shards_placement
```

Copy the `cluster-state-id` from the response headers.

Copy the JSON response body, which represents the new shard placement blueprint.

[Rearrange the database shards](/docs/latest/operate/rs/references/rest-api/requests/bdbs/actions/optimize_shards_placement/#put-bdbs-rearrange-shards) according to the new shard placement blueprint:

In the request headers, include the `cluster-state-id` from the `optimize_shards_placement` response.

Add the following JSON in the request body and replace `<shard placement blueprint>` with the new blueprint:

```sh
{
  "shards_blueprint": <shard placement blueprint>
}
```

Send the following `PUT` request to rearrange the shards:

```sh
PUT /v1/bdbs/<database-ID>
```

## Shard placement without rack-zone awareness

Even if a database has rack-zone awareness turned off, the cluster still ensures that primary and replica shards are placed on distinct nodes.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page