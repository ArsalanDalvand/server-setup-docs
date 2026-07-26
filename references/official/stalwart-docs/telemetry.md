---
title: "Overview"
source: "https://stalw.art/docs/telemetry"
source_format: official-html-to-markdown
language: "en"
doc_slug: "telemetry"
category: "stalwart"
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

      

Telemetry covers the collection, transmission, and analysis of data from a running system, used to monitor performance, diagnose issues, and guide operational decisions. In Stalwart, telemetry provides the signals administrators rely on to understand system behaviour, track performance, and validate reliability and security.

Stalwart exposes several mechanisms for capturing logs, traces, and metrics. The available telemetry facilities are:

[[telemetry/tracing|Tracing and logging]]: detailed trace and log output, configured through the [[ref/object/tracer|Tracer]] object. Output can go to a log file, standard error, the systemd journal, or an OpenTelemetry collector.
[[telemetry/metrics|Metrics]]: quantitative measurements of server activity and resource use, configured through the [[ref/object/metrics|Metrics]] singleton.
[[telemetry/webhooks|Webhooks]]: HTTP callbacks that deliver event notifications to external systems, configured through the [[ref/object/web-hook|WebHook]] object.
[[telemetry/live|Live telemetry]]: real-time streaming of events and metrics over HTTP using Server-Sent Events.
[[telemetry/alerts|Alerts]]: rules that fire when a metric condition is met, configured through the [[ref/object/alert|Alert]] object.
[[telemetry/history|History]]: persisted traces and metrics retained for later analysis, configured through the [[ref/object/tracing-store|TracingStore]] and [[ref/object/metrics-store|MetricsStore]] singletons together with the [[ref/object/data-retention|DataRetention]] object.

These mechanisms let administrators observe the operational state of Stalwart, identify and resolve issues, and tune performance.