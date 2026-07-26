---
title: "Management"
source: "https://stalw.art/docs/mta/management"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/management"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "Management"
---
# Management

      

The outbound message queue and the inbound and outbound report archives are runtime management objects, not configuration. They are created by the MTA as messages are queued and as reports are sent, received, and analysed, and are then available for inspection through the [[management/webui|WebUI]], the [[management/cli|CLI]], and the JMAP API. The existing pages under [[mta/reports|Reports]] cover report emission and analysis; this page covers access to the records themselves.

## Outbound queue

[Section titled “Outbound queue”](#outbound-queue)

Messages waiting for delivery are represented by the [[ref/object/queued-message|QueuedMessage]] object (found in the WebUI under  Management ›  Emails › Queued). Each entry carries the envelope, per-recipient delivery status, the next retry and notification timestamps, and a reference to the stored message content. Administrators can list, inspect, requeue, and cancel queued messages through this object.

## Inbound reports

[Section titled “Inbound reports”](#inbound-reports)

Reports received from other organisations are stored for review and trend analysis. The relevant objects are:

[[ref/object/arf-external-report|ArfExternalReport]] (found in the WebUI under  Management ›  Reports › Inbox › ARF): ARF feedback reports, including abuse complaints, fraud notifications, and authentication-failure reports received from external senders.
[[ref/object/dmarc-external-report|DmarcExternalReport]] (found in the WebUI under  Management ›  Reports › Inbox › DMARC): DMARC aggregate reports returned by remote receivers, summarising SPF and DKIM results for messages claiming to come from the local domains.
[[ref/object/tls-external-report|TlsExternalReport]] (found in the WebUI under  Management ›  Reports › Inbox › TLS): TLS aggregate reports describing the outcome of STARTTLS connections observed by remote servers when delivering mail to the local domains.

Inbound analysis of these reports is configured on the [[ref/object/report-settings|ReportSettings]] singleton; see [[mta/reports/analysis|Analysis]] for the intercept addresses and forwarding behaviour.

## Outbound reports

[Section titled “Outbound reports”](#outbound-reports)

Reports generated locally and waiting to be sent are retained until the aggregation window closes and delivery completes. The relevant objects are:

[[ref/object/dmarc-internal-report|DmarcInternalReport]] (found in the WebUI under  Management ›  Reports › Outbox › DMARC): pending outbound DMARC aggregate reports, accumulated from local authentication results and scheduled for delivery to the addresses listed in each domain’s DMARC policy.
[[ref/object/tls-internal-report|TlsInternalReport]] (found in the WebUI under  Management ›  Reports › Outbox › TLS): pending outbound TLS aggregate reports, accumulated from observed STARTTLS outcomes and scheduled for delivery to the policy identifiers declared by remote domains.

Emission of outbound reports is configured through the [[mta/reports/dmarc|DMARC]] and [[mta/reports/tls|TLS]] report pages; this page covers only the management of records that are already in flight.