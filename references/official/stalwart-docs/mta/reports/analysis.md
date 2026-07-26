---
title: "Analysis"
source: "https://stalw.art/docs/mta/reports/analysis"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/reports/analysis"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Analysis"
---
# Analysis

      

Stalwart automatically analyses incoming DMARC, DKIM, SPF, and TLS reports sent by other domains, removing the need for manual intervention and saving time for administrators. If TLS or message-authentication issues are detected, an event is recorded in the log file or sent to [[telemetry/tracing/opentelemetry|OpenTelemetry]]. Turning reports into actionable events allows administrators to detect and respond to configuration errors and abuse (such as spam or phishing), helping maintain the integrity of the email system.

## Settings

[Section titled “Settings”](#settings)

Inbound analysis is configured on the [[ref/object/report-settings|ReportSettings]] singleton (found in the WebUI under  Settings ›  MTA › Reports › General):

[[ref/object/report-settings#inboundreportaddresses|`inboundReportAddresses`]]: list of addresses (with optional wildcards) from which reports are intercepted and analysed. These addresses must be routable. Default `["postmaster@*"]`.
[[ref/object/report-settings#inboundreportforwarding|`inboundReportForwarding`]]: whether reports are forwarded to their final recipient after analysis. Default `true`.

Example intercepting `dmarc@*` and `abuse@*` while still forwarding the report to the original recipient:

```
{  "inboundReportAddresses": ["dmarc@*", "abuse@*"],  "inboundReportForwarding": true}
```

## Retention

[Section titled “Retention”](#retention)

How long intercepted reports are kept is controlled globally by the [[ref/object/data-retention|DataRetention]] singleton (found in the WebUI under  Settings ›  Storage › Data Retention › Archiving,  Settings ›  Storage › Data Retention › Data Cleanup,  Settings ›  Storage › Data Retention › Auto-Expunge,  Settings ›  Storage › Data Retention › Telemetry) through the [[ref/object/data-retention#holdmtareportsfor|`holdMtaReportsFor`]] field, which accepts a duration or `null` to disable storage. The default is `30d`.