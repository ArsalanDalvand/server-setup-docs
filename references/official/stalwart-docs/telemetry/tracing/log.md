---
title: "Log file"
source: "https://stalw.art/docs/telemetry/tracing/log"
source_format: official-html-to-markdown
language: "en"
doc_slug: "telemetry/tracing/log"
category: "telemetry"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Log file"
---
# Log file

      

Log-file output writes entries to a text file on disk. The file can be rotated at a fixed frequency so that long-running installations do not accumulate a single unbounded log.

Log-file output is represented by the `Log` variant of the [[ref/object/tracer|Tracer]] object (found in the WebUI under  Settings ›  Telemetry › Tracers). Alongside the common tracer fields ([[ref/object/tracer#enable|`enable`]], [[ref/object/tracer#level|`level`]], [[ref/object/tracer#lossy|`lossy`]], [[ref/object/tracer#events|`events`]], [[ref/object/tracer#eventspolicy|`eventsPolicy`]]), the variant carries:

[[ref/object/tracer#path|`path`]]: directory in which the log files are written. Required.
[[ref/object/tracer#prefix|`prefix`]]: filename prefix for each log file. Default `"stalwart"`.
[[ref/object/tracer#rotate|`rotate`]]: rotation frequency. One of `daily`, `hourly`, `minutely`, or `never`. Default `daily`.
[[ref/object/tracer#ansi|`ansi`]]: whether ANSI escape sequences are used to colour the output. Default `true`.
[[ref/object/tracer#multiline|`multiline`]]: whether each entry spans multiple lines. Default `false`.

For example:

```
{  "@type": "Log",  "path": "/opt/stalwart/logs",  "prefix": "stalwart.log",  "rotate": "daily",  "ansi": true,  "multiline": false,  "enable": true,  "level": "info"}
```