---
title: "OpenTelemetry"
source: "https://stalw.art/docs/telemetry/metrics/opentelemetry"
source_format: official-html-to-markdown
language: "en"
doc_slug: "telemetry/metrics/opentelemetry"
category: "telemetry"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "OpenTelemetry"
---
# OpenTelemetry

      

OpenTelemetry is an open-source observability framework that standardises how metrics, logs, and traces are collected, processed, and exported. It defines a common wire format for telemetry data and provides SDKs and collector services for a wide range of programming languages and platforms.

Stalwart pushes metrics to an OpenTelemetry collector, from which they can be fanned out to backends such as Prometheus, Jaeger, or Zipkin. Pushing metrics through OpenTelemetry lets Stalwart fit into an existing observability stack with a single collector configuration.

## Configuration

[Section titled “Configuration”](#configuration)

The OpenTelemetry metrics exporter is configured through [[ref/object/metrics#opentelemetry|`openTelemetry`]] on the [[ref/object/metrics|Metrics]] singleton (found in the WebUI under  Settings ›  Telemetry › Metrics › General,  Settings ›  Telemetry › Metrics › OpenTelemetry,  Settings ›  Telemetry › Metrics › Prometheus). The field is a multi-variant nested type:

`Disabled` turns the exporter off.
`Http` exports metrics over HTTP.
`Grpc` exports metrics over gRPC.

Both active variants share the following fields:

[[ref/object/metrics#metricsotelhttp|`endpoint`]]: collector endpoint URL. Required for `Http`; optional for `Grpc` (when omitted, the SDK default endpoint is used).
[[ref/object/metrics#metricsotelhttp|`interval`]]: minimum interval between push requests. Default `"1m"`.
[[ref/object/metrics#metricsotelhttp|`timeout`]]: maximum time to wait for a response. Default `"10s"`.

The `Http` variant additionally carries:

[[ref/object/metrics#httpauth|`httpAuth`]]: HTTP authentication used by the exporter. A nested type with variants `Unauthenticated`, `Basic`, and `Bearer`.
[[ref/object/metrics#metricsotelhttp|`httpHeaders`]]: additional HTTP headers sent on each request.

## Example

[Section titled “Example”](#example)

The equivalent of pushing over gRPC every thirty seconds to a collector at `https://otel-collector.example.com/v1/metrics`:

```
{  "openTelemetry": {    "@type": "Grpc",    "endpoint": "https://otel-collector.example.com/v1/metrics",    "interval": "30s"  }}
```