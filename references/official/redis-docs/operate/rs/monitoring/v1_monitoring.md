---
title: "Metrics and alerts for monitoring v1"
source: "https://redis.io/docs/latest/operate/rs/monitoring/v1_monitoring/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/monitoring/v1_monitoring"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Metrics and alerts for monitoring v1"
---
# Metrics and alerts for monitoring v1

Monitor Redis Software clusters and databases using internal monitoring systems and external monitoring tools.

            Redis Software
          

            Redis Enterprise for Kubernetes
          

## Current monitoring system (deprecated)

The current monitoring system, which is deprecated as of Redis Software version 7.22, consists of the following components:

Internal metrics storage:

Metrics are internally aggregated, calculated, and stored for up to one year.

This historical data is used to generate trends and performance insights over time.

[Statistics APIs](/docs/latest/operate/rs/references/rest-api/objects/statistics/):

This set of RESTful APIs exposes metrics collected at regular intervals from clusters, nodes, databases, shards, and endpoints.

These APIs allow customers to retrieve performance and usage statistics directly from the internal storage layer.

Cluster manager metrics and alerts:

The Cluster Manager UI includes [dedicated metrics pages](#cluster-manager-metrics) that display pre-aggregated metrics.

[Cluster alerts](#cluster-alerts) are triggered based on thresholds applied to these stored metrics.

v1 Prometheus scraping endpoint:

Redis Software exposes a legacy `/prometheus_metrics` endpoint to integrate with external observability platforms like [[operate/rs/monitoring/get-started|Prometheus and Grafana]].

This endpoint fetches data from the internal storage, providing basic monitoring integration.

### Limitations

The internal monitoring system, while functional, has several limitations that affect scalability and accuracy:

Limited granularity: Metrics are aggregated before storage, resulting in a loss of fine-grained insights.

Stale data: Stored metrics can lag behind real-time system states, reducing the effectiveness of alerting.

Visibility gaps during management operations: Metrics and statistics may not be available or updated during management tasks, resulting in incomplete monitoring during critical activities.

Scalability constraints: Internal storage and processing introduce performance overhead and are not optimized for large-scale observability pipelines.

Limited extensibility: The system is tightly coupled with internal components, making it difficult to integrate with modern monitoring ecosystems.

### Transition to the metrics stream engine

To improve monitoring and address current limitations, Redis Software is transitioning to a new observability foundation: the [[operate/rs/monitoring/metrics_stream_engine|metrics stream engine]].

This modern monitoring stack introduces:

Real-time metrics, exposed directly from the engine without intermediate storage for high-fidelity, low-latency insights.

Scalable architecture designed for cloud-native observability with lightweight Prometheus collectors.

Deeper visibility by exposing new types of metrics such as key size distribution, server overall latency histograms, and system internals with per-endpoint resolution.

We recommend migrating to the metrics stream engine for enhanced accuracy, scalability, and future-proof observability.

If you are already using the existing scraping endpoint for integration, follow [[operate/rs/monitoring/metrics_stream_engine/prometheus-metrics-v1-to-v2|this guide]] to transition and try the new engine. It is possible to scrape both existing and new endpoints simultaneously, allowing advanced dashboard preparation and a smooth transition.

### Transition cluster manager alerts

As part of Redis Software's transition to the [[operate/rs/monitoring/metrics_stream_engine|new metrics stream engine]], some internal cluster manager alerts were deprecated in favor of external monitoring solutions. See the [[operate/rs/monitoring/metrics_stream_engine/alerts-v1-to-v2|alerts transition plan]] for guidance.

## Cluster manager metrics

You can see the metrics of the cluster in:

Cluster > Metrics
Node > Metrics for each node
Database > Metrics for each database, including the shards for that database

The scale selector at the top of the page allows you to set the X-axis (time) scale of the graph.

To choose which metrics to display in the two large graphs at the top of the page:

Hover over the graph you want to show in a large graph.
Click on the right or left arrow to choose which side to show the graph.

We recommend that you show two similar metrics in the top graphs so you can compare them side-by-side.

See the following topics for metrics definitions:

[Database operations](/docs/latest/operate/rs/references/metrics/database-operations/) for database metrics
[Resource usage](/docs/latest/operate/rs/references/metrics/resource-usage/) for resource and database usage metrics
[Auto Tiering](/docs/latest/operate/rs/references/metrics/auto-tiering/) for additional metrics for [Auto Tiering](/docs/latest/operate/rs/databases/flash/) databases

## Cluster alerts

In ClusterÂ >Â Alert Settings, you can enable alerts for node or cluster events, such as high memory usage or throughput.

Configured alerts are shown:

As a notification on the status icon ( 

	

 ) for the node and cluster
In the log
In email notifications, if you configure [email alerts](#send-alerts-by-email)

Note:
  If you enable alerts for "Node joined" or "Node removed" actions,
you must also enable "ReceiveÂ email alerts" so that the notifications are sent.

To enable alerts for a cluster:

In Cluster > Alert Settings, click Edit.
Select the alerts that you want to show for the cluster and click Save.

## Database alerts

For each database, you can enable alerts for database events, such as high memory usage or throughput.

Configured alerts are shown:

As a notification on the status icon ( 

	

 ) for the database
In the log
In emails, if you configure [email alerts](#send-alerts-by-email)

To enable alerts for a database:

In Configuration for the database, click Edit.
Select the Alerts section to open it.
Select the alerts that you want to show for the database and click Save.

## Send alerts by email

To send cluster and database alerts by email:

In Cluster > Alert Settings, click Edit.
Select Set an email to configure the [[operate/rs/clusters/configure/cluster-settings#configuring-email-server-settings|email server settings]].
In Configuration for the database, click Edit.
Select the Alerts section to open it.
Select Receive email alerts and click Save.
In Access Control, select the [[operate/rs/security/access-control/manage-users|database and cluster alerts]] that you want each user to receive.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page