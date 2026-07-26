---
title: "Overview"
source: "https://stalw.art/docs/telemetry/tracing"
source_format: official-html-to-markdown
language: "en"
doc_slug: "telemetry/tracing"
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

      

Stalwart produces detailed tracing and logging output that can be directed to several destinations in parallel, each with its own severity threshold and event filters. Multiple tracers can be configured side by side; the destination and filter choices determine what each consumer receives.

Each tracer is an instance of the [[ref/object/tracer|Tracer]] object (found in the WebUI under  Settings ›  Telemetry › Tracers). The object is multi-variant: the chosen variant selects the output method and the fields it carries, while a set of common fields ([[ref/object/tracer#enable|`enable`]], [[ref/object/tracer#level|`level`]], [[ref/object/tracer#lossy|`lossy`]], [[ref/object/tracer#events|`events`]], [[ref/object/tracer#eventspolicy|`eventsPolicy`]]) apply to every variant.

The supported variants are:

[[telemetry/tracing/opentelemetry|OpenTelemetry]]: sends traces and logs to an OpenTelemetry collector over HTTP or gRPC. Two variants, `OtelHttp` and `OtelGrpc`, cover the two transports.
[[telemetry/tracing/log|Log file]]: writes entries to a text file with rotation (`Log` variant).
[[telemetry/tracing/journal|Journal]]: forwards entries to the systemd journal (`Journal` variant). Linux only.
[[telemetry/tracing/console|Console]]: prints entries to standard error (`Stdout` variant).

## Logging levels

[Section titled “Logging levels”](#logging-levels)

The verbosity of a tracer is set through [[ref/object/tracer#level|`level`]]. The available levels are:

`error`: only critical errors that threaten normal operation.
`warn`: warnings that indicate potential problems.
`info`: general informational output.
`debug`: detailed diagnostic information useful when troubleshooting.
`trace`: the most verbose level, logging nearly every internal operation.

A tracer can be switched off without deleting it by setting [[ref/object/tracer#enable|`enable`]] to `false`.

## Event filters

[Section titled “Event filters”](#event-filters)

Each tracer can be narrowed to a specific set of events through [[ref/object/tracer#events|`events`]] together with [[ref/object/tracer#eventspolicy|`eventsPolicy`]]. The policy is either `include` (only the listed events are emitted) or `exclude` (the listed events are suppressed and everything else is emitted). The list of event identifiers is documented on the [[telemetry/events|Events]] page.

## Backpressure

[Section titled “Backpressure”](#backpressure)

If a tracer cannot keep up with the emitted volume, the default behaviour is to apply backpressure. Setting [[ref/object/tracer#lossy|`lossy`]] to `true` lets the server drop entries instead, which is appropriate when loss of detail is preferable to slowing down the main event path.