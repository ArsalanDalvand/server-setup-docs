---
title: "Console"
source: "https://stalw.art/docs/telemetry/tracing/console"
source_format: official-html-to-markdown
language: "en"
doc_slug: "telemetry/tracing/console"
category: "telemetry"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Console"
---
# Console

      

The console output writes every log and trace entry to standard error. This is convenient when Stalwart runs in a container environment such as Docker, where the container runtime captures `stderr` directly.

The console output is represented by the `Stdout` variant of the [[ref/object/tracer|Tracer]] object (found in the WebUI under  Settings ›  Telemetry › Tracers). Alongside the common tracer fields ([[ref/object/tracer#enable|`enable`]], [[ref/object/tracer#level|`level`]], [[ref/object/tracer#lossy|`lossy`]], [[ref/object/tracer#events|`events`]], [[ref/object/tracer#eventspolicy|`eventsPolicy`]]), the variant carries:

[[ref/object/tracer#ansi|`ansi`]]: whether ANSI escape sequences are used to colour the output. Default `false`.
[[ref/object/tracer#multiline|`multiline`]]: whether each entry spans multiple lines. Default `false`.
[[ref/object/tracer#buffered|`buffered`]]: whether entries are buffered before being written. Default `true`.

For example:

```
{  "@type": "Stdout",  "ansi": true,  "multiline": false,  "buffered": true,  "enable": true,  "level": "trace"}
```