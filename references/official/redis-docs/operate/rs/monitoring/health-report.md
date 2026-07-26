---
title: "Consolidated health report"
source: "https://redis.io/docs/latest/operate/rs/monitoring/health-report/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/monitoring/health-report"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Consolidated health report"
---
# Consolidated health report

View the consolidated health report to get a read-only snapshot of a Redis Software cluster's current system health.

            Redis Software
          

The consolidated health report is a read-only, consolidated snapshot of a cluster's current system health, including:

License and certificate status

Nodes and services health

Node memory breakdown, including free and provisioned memory

Running actions status

Active alerts

Basic database status and version

High availability health

The consolidated health report consolidates cluster and database health information that was previously scattered across multiple APIs and CLI commands, such as [`rladmin status extra all`](/docs/latest/operate/rs/references/cli-utilities/rladmin/status/) and required SSH access to view.

As of Redis Software version 8.0.16, you can access the cluster and database health reports using the [Cluster manager UI](#view-health-overview) or [REST API requests](#health-rest-api-requests).

## View health overview

You can access the cluster and database health reports using the Overview screen in the Cluster Manager UI.

You can view node health

[](/docs/latest/images/rs/screenshots/overview/health-report-overview.png)

### Cluster info

The Cluster Info section provides basic cluster details, including:

Overall cluster status

Redis Software cluster version

License usage and expiration date

### Cluster storage

The Cluster Storage section provides memory usage details, including used memory, free memory, and total memory for RAM, flash, persistent storage, and ephemeral storage on the cluster.

### Nodes

The Nodes section shows the status of nodes in the cluster and indicates which node is the leader or primary node.

### Databases

In the Databases section, you can check each database's status and Redis version. You can click the number listed by each field to display the relevant database names and click on the database links to view database details.

[](/docs/latest/images/rs/screenshots/overview/db-status-hover.png)

### Certificates

The Certificates section shows whether the cluster's certificates are still valid. If a certificate appears as no longer valid, you can click the Certificates link to update the certificate.

[](/docs/latest/images/rs/screenshots/overview/cert-expires-soon.png)

### Actions

In the Actions section, you can view currently running actions.

[](/docs/latest/images/rs/screenshots/overview/running-actions.png)

### Alerts and services

If a configured alert threshold is reached or a running service stops, an alert will appear in the Alerts & Services section.

[](/docs/latest/images/rs/screenshots/overview/active-alerts.png)

## View node health

On the Nodes screen of the Cluster Manager UI, you can click on a node to go to its Node configuration tab and view node health details.

[](/docs/latest/images/rs/screenshots/nodes/node-config-screen.png)

### Verify nodes

You can check if nodes are functioning properly using one of the following methods:

        Verify all nodes
      

        Verify specific node
      

        On the Nodes screen, click Verify all nodes.
      

On the Nodes screen, find the node you want verify in the list and click the More actions button (â®).

Select Verify node from the list.

### Node storage

The Node storage section of the Node configuration tab provides memory usage details, including used memory, free memory, and total memory for RAM, flash, persistent storage, and ephemeral storage on the node.

### Services

In the Services section Node configuration tab, you can view the list of services running on the node and the most recent status of each.

[](/docs/latest/images/rs/screenshots/nodes/node-stopped-service.png)

### Shards

When viewing a node's configuration, you can click the Shards tab to view a list of database shards running on the node and related details such as each shard's ID, current status, role, and slot range.

[](/docs/latest/images/rs/screenshots/nodes/node-shards.png)

## Health report REST API requests

### Cluster health report requests

[Cluster health report](/docs/latest/operate/rs/references/rest-api/api-reference/#tag/Cluster/operation/cluster_cluster_health_report) REST API requests provide overall health information for a cluster:

```sh
GET https://<host>:<port>/v4/cluster/health
```

### Database health report requests

[Database health report](/docs/latest/operate/rs/references/rest-api/api-reference/#tag/Database/operation/cluster_database_health_report) REST API requests provide detailed health information for a specific database:

```sh
GET https://<host>:<port>/v4/bdb/<database_id>/health
```

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page