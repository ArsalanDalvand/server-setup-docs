---
title: "Live Telemetry"
source: "https://stalw.art/docs/telemetry/live"
source_format: official-html-to-markdown
language: "en"
doc_slug: "telemetry/live"
category: "telemetry"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Live Telemetry"
---
# Live Telemetry

      

Live telemetry exposes an HTTP endpoint that streams structured logs, traces, and metrics to the client in real time. The stream is delivered using Server-Sent Events (SSE), so any HTTP client capable of consuming an event stream can subscribe. The feature is suited to live diagnostics, short-lived performance investigations, and dashboards that need a continuous update path.

Filters can be applied on the URL to narrow the stream to specific spans, metrics, or event keys. This is important on busy servers, where the unfiltered stream can be too large to process interactively.

The [[management/webui|WebUI]] is the recommended consumer: its Dashboard formats the live stream and provides controls for filtering and visualising activity.

## Tracing endpoint

[Section titled “Tracing endpoint”](#tracing-endpoint)

Real-time tracing and log data is available at `/api/telemetry/traces/live`. The endpoint returns a continuous SSE stream; each event is a JSON object describing a server [[telemetry/events|event]], carrying structured keys that identify the affected accounts, connections, protocols, and other context.

### Parameters

[Section titled “Parameters”](#parameters)

Filters narrow the stream by matching on event keys. They can be applied broadly or per key:

`filter`: matches the given value across every structured key. Useful for a quick search. For example, `/api/telemetry/traces/live?filter=error`.
Per-key parameters: any structured [[telemetry/events#key-types|key]] can be used as a query parameter to match on that key only. Multiple constraints combine. For example, `/api/telemetry/traces/live?remote-ip=192.168.1.1&domain=example.org`.

### Response format

[Section titled “Response format”](#response-format)

Each line is a JSON object. The available keys depend on the event type; the full key catalogue is documented on the [[telemetry/events#key-types|Events]] page.

## Metrics endpoint

[Section titled “Metrics endpoint”](#metrics-endpoint)

Real-time metrics are available at `/api/telemetry/metrics/live`. The endpoint returns a continuous SSE stream of metric samples, each encoded as a JSON object that describes a single data point.

### Parameters

[Section titled “Parameters”](#parameters-1)

`metrics`: comma-separated list of metric names to include in the stream. For example, `/api/telemetry/metrics/live?metrics=server.memory,queue.count`.
`interval`: sampling interval in seconds. For example, `/api/telemetry/metrics/live?interval=30` samples every thirty seconds.

### Response format

[Section titled “Response format”](#response-format-1)

Each line is a JSON object with structured keys describing the metric and its current value.