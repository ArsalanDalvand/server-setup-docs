---
title: "Metrics configuration"
source: "https://redis.io/docs/latest/operate/rs/monitoring/metrics_stream_engine/metrics-configuration/"
source_format: official-html-to-markdown
language: "en"
doc_slug: "operate/rs/monitoring/metrics_stream_engine/metrics-configuration"
category: "operate"
retrieved_at: "2026-07-26T15:48:53+00:00"
tags:
  - redis
  - cache-store
  - official-documentation
  - in-memory-db
aliases:
  - "Metrics configuration"
---
# Metrics configuration

Configure the Redis Software v2 metrics stream engine.

The metrics configuration is a cluster-wide resource that controls the behavior of the [[operate/rs/monitoring/metrics_stream_engine|v2 metrics stream engine]], such as which optional metrics are collected and how metrics are stored and exported.

For the full list of configuration fields, their types, and defaults, see the [metrics configuration object](/docs/latest/operate/rs/references/rest-api/objects/metrics_config/).

## Configure metrics settings

You can read and update the metrics configuration through the REST API or with `rladmin`.

Changes to the metrics configuration are recorded in the cluster's [[operate/rs/clusters/logging|event logs]], so you can review who changed a setting and when.

### REST API

Use the `GET` and `PUT` `/v1/metrics_config` requests to read and update the configuration. For request and response details, required permissions, and status codes, see [Metrics configuration requests](/docs/latest/operate/rs/references/rest-api/requests/metrics_config/).

### rladmin

Use [`rladmin info metrics`](/docs/latest/operate/rs/references/cli-utilities/rladmin/info/#info-metrics) to view the current configuration and [`rladmin metrics config`](/docs/latest/operate/rs/references/cli-utilities/rladmin/metrics/#metrics-config) to update it.

RATE THIS PAGE

★

★

★

★

★

[Back to top â](#)

## On this page