---
title: "Monitoring with metrics and alerts"
source: "https://redis.io/docs/latest/operate/rs/monitoring/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/monitoring"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Monitoring with metrics and alerts"
---
# Monitoring with metrics and alerts

Use the metrics that measure the performance of your Redis Software clusters, nodes, databases, and shards to track the performance of your databases.

            Redis Software
          

            Redis Enterprise for Kubernetes
          

You can use the metrics that measure the performance of your Redis Software clusters, nodes, databases, and shards
to monitor the performance of your databases.

## View metrics and configure alerts

In the Redis Software Cluster Manager UI, you can view metrics, configure alerts, and send notifications based on alert parameters. You can also access metrics and configure alerts through the REST API.

See [[operate/rs/monitoring/v1_monitoring|Metrics and alerts for monitoring v1]] for more information.

## Metrics stream engine

The new metrics stream engine is generally available as of [Redis Software version 8.0](/docs/latest/operate/rs/release-notes/rs-8-0-releases/) This new engine exposes the v2 Prometheus scraping endpoint at `https://<IP>:8070/v2`, exports all time-series metrics to external monitoring tools, and enables real-time monitoring.

See [[operate/rs/monitoring/metrics_stream_engine|Metrics stream engine for monitoring v2]] for more information.

## Integrate with external monitoring tools

To integrate Redis Software metrics into your monitoring environment, see the following integration guides:

[Grafana](/docs/latest/integrate/prometheus-with-redis-enterprise/)

[Datadog](/docs/latest/integrate/datadog-with-redis-enterprise/)

[Dynatrace](/docs/latest/integrate/dynatrace-with-redis-enterprise/)

[New Relic](/docs/latest/integrate/new-relic-with-redis-enterprise/)

For a detailed tutorial to deploy a complete monitoring stack with Prometheus and Grafana, see [Redis Software Observability with Prometheus and Grafana](https://redis.io/learn/operate/observability/redis-software-prometheus-and-grafana).

Filter [[integrate|Libraries and tools]] by "observability" for additional tools and guides.

## Metrics reference

Make sure you read the [definition of each metric](/docs/latest/operate/rs/references/metrics/)
so that you understand exactly what it represents.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page