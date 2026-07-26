---
title: "History"
source: "https://stalw.art/docs/telemetry/history"
source_format: official-html-to-markdown
language: "en"
doc_slug: "telemetry/history"
category: "telemetry"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "History"
---
# History

      

Telemetry history retains selected event spans (for example, incoming messages and completed deliveries) and periodic metric samples in a dedicated database, so that past activity can be reviewed after the fact. Unlike [[telemetry/live|live telemetry]], which streams data to connected clients in real time, history persists the signal to storage for later query.

Stored spans and metrics can be retrieved through the JMAP API, which exposes filtering on time range, event type, and other criteria. The same data drives the `Dashboard` and `History` views in the [[management/webui|WebUI]].

## Tracing history

[Section titled “Tracing history”](#tracing-history)

Tracing history is controlled through the [[ref/object/tracing-store|TracingStore]] singleton (found in the WebUI under  Settings ›  Storage › Tracing Store). The object is multi-variant: the chosen variant selects the backend used to store spans. Selecting `Disabled` turns tracing history off; selecting `Default` reuses the main [[storage/data|data store]]; other variants (such as `FoundationDb` or `PostgreSql`) point history at a dedicated backend.

Retention is controlled by [[ref/object/data-retention#holdtracesfor|`holdTracesFor`]] on the [[ref/object/data-retention|DataRetention]] object (found in the WebUI under  Settings ›  Storage › Data Retention › Archiving,  Settings ›  Storage › Data Retention › Data Cleanup,  Settings ›  Storage › Data Retention › Auto-Expunge,  Settings ›  Storage › Data Retention › Telemetry). The value is a duration, for example `"7d"`, `"12h"`, or `"30m"`. The default is `"30d"`.

For example, to retain delivery traces in the main data store for thirty days, set the `TracingStore` singleton to `{"@type": "Default"}` and leave [[ref/object/data-retention#holdtracesfor|`holdTracesFor`]] at its default.

## Metrics history

[Section titled “Metrics history”](#metrics-history)

Metrics history is controlled through the [[ref/object/metrics-store|MetricsStore]] singleton (found in the WebUI under  Settings ›  Storage › Metrics Store). The object is multi-variant in the same way as `TracingStore`: `Disabled` turns metrics history off, `Default` reuses the main data store, and the backend-specific variants point history at a dedicated store.

Retention and sampling frequency are set on the [[ref/object/data-retention|DataRetention]] object:

[[ref/object/data-retention#holdmetricsfor|`holdMetricsFor`]]: duration for which metrics are retained. Default `"90d"`.
[[ref/object/data-retention#metricscollectioninterval|`metricsCollectionInterval`]]: how often metrics are sampled. Specified as a cron expression. The default samples once per hour at minute zero.