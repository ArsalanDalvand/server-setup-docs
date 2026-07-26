---
title: "Overview"
source: "https://stalw.art/docs/telemetry/metrics"
source_format: official-html-to-markdown
language: "en"
doc_slug: "telemetry/metrics"
category: "telemetry"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Overview"
---
# Overview

      

Metrics are quantitative measurements of the server’s operation, resource use, and workload. They let administrators see how the server behaves over time, spot trends, and react to anomalies. Metric collection in Stalwart is controlled by the [[ref/object/metrics|Metrics]] singleton (found in the WebUI under  Settings ›  Telemetry › Metrics › General,  Settings ›  Telemetry › Metrics › OpenTelemetry,  Settings ›  Telemetry › Metrics › Prometheus).

## Push and pull exporters

[Section titled “Push and pull exporters”](#push-and-pull-exporters)

Two export models are supported:

Push: the server actively sends metric samples to a collection system at regular intervals. Push is simple to configure, and metrics arrive at the collector as they are produced.
Pull: the collection system queries an HTTP endpoint on the server at a cadence it controls. Pull lets the collector manage the scraping load and is the model used by tools such as Prometheus.

## Supported backends

[Section titled “Supported backends”](#supported-backends)

Stalwart supports both models through a single configuration surface:

[[telemetry/metrics/opentelemetry|OpenTelemetry]] is used for push. It is configured through [[ref/object/metrics#opentelemetry|`openTelemetry`]] on the `Metrics` singleton, which is a nested type with variants `Disabled`, `Http`, and `Grpc`.
[[telemetry/metrics/prometheus|Prometheus]] is used for pull. It is configured through [[ref/object/metrics#prometheus|`prometheus`]] on the `Metrics` singleton, which is a nested type with variants `Disabled` and `Enabled`.

Both exporters can be configured at the same time when two independent collectors need the same signal.

## Selecting metrics

[Section titled “Selecting metrics”](#selecting-metrics)

The set of metrics exported is controlled by [[ref/object/metrics|`metrics`]] and [[ref/object/metrics#metricspolicy|`metricsPolicy`]] on the `Metrics` singleton. With [[ref/object/metrics#metricspolicy|`metricsPolicy`]] set to `exclude` (the default), the listed metrics are suppressed and everything else is emitted; with `include`, only the listed metrics are emitted.

For example, to suppress the noise of `auth.error` and `smtp.error` while leaving every other metric enabled:

```
{  "metrics": ["auth.error", "smtp.error"],  "metricsPolicy": "exclude"}
```