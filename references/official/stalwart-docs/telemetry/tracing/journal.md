---
title: "Journal"
source: "https://stalw.art/docs/telemetry/tracing/journal"
source_format: official-html-to-markdown
language: "en"
doc_slug: "telemetry/tracing/journal"
category: "telemetry"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Journal"
---
# Journal

      

The journal is the logging facility provided by the `systemd` suite, managed by the `systemd-journald` service. Unlike text-file logging (for example `/var/log/syslog` or `/var/log/messages`), the journal captures system and service messages in a binary format, which supports indexed queries and integrity checks.

Journal data is stored in journal files. By default these live at:

`/run/log/journal/` for volatile logs, which are held in memory and lost on reboot.
`/var/log/journal/` for persistent logs, which are written to disk and survive reboots.

If `/var/log/journal/` exists, logs are persistent; otherwise they remain volatile. The binary format means the content cannot be read with tools such as `cat` or `less`; the `journalctl` command is used to query and display entries.

Journal output is represented by the `Journal` variant of the [[ref/object/tracer|Tracer]] object (found in the WebUI under  Settings ›  Telemetry › Tracers). The variant carries no variant-specific fields; only the common tracer fields apply ([[ref/object/tracer#enable|`enable`]], [[ref/object/tracer#level|`level`]], [[ref/object/tracer#lossy|`lossy`]], [[ref/object/tracer#events|`events`]], [[ref/object/tracer#eventspolicy|`eventsPolicy`]]).

For example:

```
{  "@type": "Journal",  "enable": true,  "level": "info"}
```

The `Journal` variant is only available on Linux systems running `systemd`.