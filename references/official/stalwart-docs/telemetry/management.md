---
title: "Management"
source: "https://stalw.art/docs/telemetry/management"
source_format: official-html-to-markdown
language: "en"
doc_slug: "telemetry/management"
category: "telemetry"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Management"
---
# Management

      

Traces, metrics, and log entries are runtime telemetry records produced by the server and retained according to the configured telemetry history. The existing [[telemetry/live|Live telemetry]] page covers real-time streaming, and the [[telemetry/history|History]] page covers the backends and retention windows that persist this data. Once persisted, the records themselves are accessible as management objects through the [[management/webui|WebUI]], the [[management/cli|CLI]], and the JMAP API.

## Traces

[Section titled “Traces”](#traces)

A [[ref/object/trace|Trace]] (found in the WebUI under  Management ›  Emails › History › Inbound Delivery,  Management ›  Emails › History › Outbound Delivery) represents a single message-delivery trace with its associated events: inbound reception, queueing, outbound delivery attempts, and the outcome of each hop. Traces are retrievable by id, filterable by account, remote host, domain, and time range, and are primarily surfaced through the `History` view of the WebUI. Retention is governed by [[ref/object/data-retention#holdtracesfor|`holdTracesFor`]] on the [[ref/object/data-retention|DataRetention]] object.

## Metrics

[Section titled “Metrics”](#metrics)

A [[ref/object/metric|Metric]] is a single sampled data point of a server metric (counter, gauge, or histogram variants). Samples are collected on the schedule defined by [[ref/object/data-retention#metricscollectioninterval|`metricsCollectionInterval`]] and retained for the duration given by [[ref/object/data-retention#holdmetricsfor|`holdMetricsFor`]]. Queries support filtering by metric name and time range and drive the Dashboard charts in the WebUI.

Individual samples are consumed by the Dashboard in the WebUI and do not have a dedicated inspection surface; administrators who need raw samples can fetch them through the [[management/cli|CLI]] or the JMAP API.

## Logs

[Section titled “Logs”](#logs)

A [[ref/object/log|Log]] (found in the WebUI under  Management ›  Observability › Logs) is a server log entry captured by the tracing subsystem and persisted to the history store. Unlike traces and metrics, log access is available in the Community Edition. Entries carry a timestamp, level, and the structured keys documented on the [[telemetry/events|Events]] page, and can be queried through the same management surfaces.