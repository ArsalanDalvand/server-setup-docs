---
title: "Prometheus"
source: "https://stalw.art/docs/telemetry/metrics/prometheus"
source_format: official-html-to-markdown
language: "en"
doc_slug: "telemetry/metrics/prometheus"
category: "telemetry"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Prometheus"
---
# Prometheus

      

Prometheus is an open-source monitoring and alerting toolkit developed originally at SoundCloud and now part of the Cloud Native Computing Foundation. It uses a time-series data model, a dedicated query language (PromQL), and a pull-based scraping model where the server collects metrics from each monitored target at regular intervals.

Stalwart exposes a Prometheus-compatible scraping endpoint so that a Prometheus instance (or any Prometheus-format scraper) can collect metrics directly. From there, metrics can be queried with PromQL, visualised in Grafana, or used to drive alerting rules.

## Scraping endpoint

[Section titled “Scraping endpoint”](#scraping-endpoint)

When the Prometheus exporter is enabled, metrics are exposed under `/metrics/prometheus`. A Prometheus job definition should point at this path on the Stalwart host and set the scrape interval required by the monitoring setup.

## Configuration

[Section titled “Configuration”](#configuration)

The Prometheus exporter is configured through [[ref/object/metrics#prometheus|`prometheus`]] on the [[ref/object/metrics|Metrics]] singleton (found in the WebUI under  Settings ›  Telemetry › Metrics › General,  Settings ›  Telemetry › Metrics › OpenTelemetry,  Settings ›  Telemetry › Metrics › Prometheus). The field is a multi-variant nested type with two variants:

`Disabled` turns the endpoint off.
`Enabled` exposes `/metrics/prometheus` and accepts optional Basic authentication credentials.

The `Enabled` variant carries:

[[ref/object/metrics#metricsprometheusproperties|`authUsername`]]: optional username for Basic authentication.
[[ref/object/metrics#metricsprometheusproperties|`authSecret`]]: optional secret for Basic authentication. A `SecretKeyOptional` with variants `None`, `Value`, `EnvironmentVariable`, and `File`.

Leaving both credentials unset exposes the endpoint without authentication.

## Example

[Section titled “Example”](#example)

The equivalent of enabling the endpoint with Basic authentication credentials `prometheus` / `password123`:

```
{  "prometheus": {    "@type": "Enabled",    "authUsername": "prometheus",    "authSecret": {"@type": "Value", "secret": "password123"}  }}
```