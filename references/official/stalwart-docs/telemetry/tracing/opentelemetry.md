---
title: "OpenTelemetry"
source: "https://stalw.art/docs/telemetry/tracing/opentelemetry"
source_format: official-html-to-markdown
language: "en"
doc_slug: "telemetry/tracing/opentelemetry"
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

      

OpenTelemetry is an open-source standard for distributed tracing and metrics. It defines a common set of APIs, SDKs, and collector services that applications use to emit traces, logs, and metrics to an observability backend.

Directing Stalwart’s traces and logs to an OpenTelemetry collector lets them be correlated with signals from the rest of the infrastructure in a single observability stack, which helps with diagnosing distributed issues and monitoring the health of the mail service.

OpenTelemetry output is represented by two variants of the [[ref/object/tracer|Tracer]] object (found in the WebUI under  Settings ›  Telemetry › Tracers): `OtelHttp` for the HTTP transport and `OtelGrpc` for the gRPC transport. Both variants share the same set of fields:

[[ref/object/tracer#endpoint|`endpoint`]]: endpoint URL of the collector. Required for `OtelHttp`; optional for `OtelGrpc` (when omitted, the SDK default endpoint is used).
[[ref/object/tracer#enablelogexporter|`enableLogExporter`]]: whether logs are exported. Default `true`.
[[ref/object/tracer#enablespanexporter|`enableSpanExporter`]]: whether spans are exported. Default `true`.
[[ref/object/tracer#throttle|`throttle`]]: minimum interval between batches. Default `"1s"`.
[[ref/object/tracer#timeout|`timeout`]]: maximum time to wait for a response. Default `"10s"`.
[[ref/object/tracer#httpauth|`httpAuth`]]: HTTP authentication used by the exporter. A nested type with variants `Unauthenticated`, `Basic`, and `Bearer`.
[[ref/object/tracer#httpheaders|`httpHeaders`]]: additional HTTP headers to include on each request.

The common tracer fields ([[ref/object/tracer#enable|`enable`]], [[ref/object/tracer#level|`level`]], [[ref/object/tracer#lossy|`lossy`]], [[ref/object/tracer#events|`events`]], [[ref/object/tracer#eventspolicy|`eventsPolicy`]]) also apply.

## gRPC transport

[Section titled “gRPC transport”](#grpc-transport)

The `OtelGrpc` variant sends traces and logs over gRPC. For example:

```
{  "@type": "OtelGrpc",  "endpoint": "https://127.0.0.1/otel",  "httpAuth": {"@type": "Unauthenticated"},  "enable": true,  "level": "info"}
```

## HTTP transport

[Section titled “HTTP transport”](#http-transport)

The `OtelHttp` variant sends traces and logs over HTTP. Use [[ref/object/tracer#httpheaders|`httpHeaders`]] for any custom headers the collector requires, and [[ref/object/tracer#httpauth|`httpAuth`]] for bearer or basic credentials. For example:

```
{  "@type": "OtelHttp",  "endpoint": "https://tracing.mydomain.test/v1/otel",  "httpAuth": {    "@type": "Bearer",    "bearerToken": {"@type": "Value", "secret": "<token>"}  },  "enable": true,  "level": "debug"}
```

Authorization tokens are supplied through [[ref/object/tracer#httpauth|`httpAuth`]] rather than as an `Authorization` entry in [[ref/object/tracer#httpheaders|`httpHeaders`]]; [[ref/object/tracer#httpheaders|`httpHeaders`]] is reserved for additional custom headers.