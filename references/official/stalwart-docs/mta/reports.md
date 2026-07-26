---
title: "Overview"
source: "https://stalw.art/docs/mta/reports"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/reports"
category: "mta"
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

      

Stalwart MTA supports both the generation and analysis of a variety of email-related reports, aiding message-delivery transparency, authentication monitoring, and transport-security compliance. These reports provide insight into the health and trustworthiness of the mail infrastructure and can be used both operationally and for long-term policy evaluation.

Stalwart can generate and send the following types of standardised reports:

[[mta/reports/dsn|Delivery Status Notifications (DSNs)]]: inform senders about delivery issues such as delays, soft failures, or permanent bounces. DSNs are sent automatically based on delivery outcomes and configuration.
[[mta/reports/dmarc#aggregate-reports|DMARC aggregate reports]]: summarise authentication results (SPF, DKIM) for messages claiming to come from a domain, typically on a daily cadence.
[[mta/reports/dmarc#failure-reports|DMARC failure reports]]: sent when a message claiming to be from a domain fails DMARC alignment. Useful to identify spoofing or unauthorised senders.
[[mta/reports/tls|TLS aggregate reports]]: summarise the success or failure of STARTTLS connections with other mail servers. Useful for detecting transport-security misconfigurations.
[[mta/reports/dkim|DKIM failure reports]]: triggered when a message fails DKIM validation. Useful for diagnosing signing or policy issues.
[[mta/reports/spf|SPF failure reports]]: sent when a message fails SPF checks. Useful to identify unauthorised senders or misconfigured SPF records.

In addition to sending reports, Stalwart can [[mta/reports/analysis|receive and automatically analyse]] a wide range of incoming email-security reports: DMARC aggregate reports, SMTP TLS aggregate reports, authentication-failure reports (DKIM, SPF, and DMARC failures), and ARF reports (Abuse Reporting Format, used for spam complaints and security incidents). Received reports are parsed and stored for visualisation within the [[management/webui|WebUI]], allowing review of authentication trends, anomaly detection, and tracking of policy-enforcement results over time.

If anomalies are detected, such as unusually high failure rates or authentication misalignments, Stalwart generates warnings forwarded to the configured [[telemetry|logging or tracing]] mechanism. Warnings can also trigger [[telemetry/webhooks|webhooks]] for integration with external monitoring systems or automated response workflows.

## Configuration

[Section titled “Configuration”](#configuration)

Shared report settings are configured on the [[ref/object/report-settings|ReportSettings]] singleton (found in the WebUI under  Settings ›  MTA › Reports › General).

### Report domain

[Section titled “Report domain”](#report-domain)

The [[ref/object/report-settings#outboundreportdomain|`outboundReportDomain`]] field specifies the default domain used as the organisational identity in outgoing reports. This domain appears in the sender address and in fields such as `Reporting-MTA` in DSNs, `reporting-org` in DMARC aggregate reports, and the domain portion of the `report-id` used for message correlation. If left empty, the default domain is used.

```
{  "outboundReportDomain": "example.org"}
```

### Submitter address

[Section titled “Submitter address”](#submitter-address)

The [[ref/object/report-settings#outboundreportsubmitter|`outboundReportSubmitter`]] field defines the hostname or identifier used in the `Submitter` field of certain report types, such as DMARC aggregate reports. This helps distinguish between MTA instances in multi-node or distributed environments. If left unset, the default expression resolves to the system hostname.

```
{  "outboundReportSubmitter": {"else": "'mx.example.org'"}}
```