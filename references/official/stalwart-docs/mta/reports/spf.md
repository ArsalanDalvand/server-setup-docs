---
title: "SPF"
source: "https://stalw.art/docs/mta/reports/spf"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/reports/spf"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "SPF"
---
# SPF

      

SPF authentication-failure reporting generates reports for sending mail servers when SPF checks fail at the receiver. The receiver verifies that an incoming message is authorised by the domain specified in the envelope (the `MAIL FROM` identity). When the check fails, a failure report is sent back to the sender, including the SPF record, the IP address of the sending server, and relevant header details. These reports help sending operators identify and correct issues with their SPF records.

Stalwart [[mta/reports/analysis|automatically analyses]] received SPF failure reports from external hosts and can also generate its own SPF reports for other hosts. Outgoing SPF failure reports are configured on the [[ref/object/spf-report-settings|SpfReportSettings]] singleton (found in the WebUI under  Settings ›  MTA › Reports › SPF):

[[ref/object/spf-report-settings#fromname|`fromName`]]: expression returning the name used in the `From` header of the report. Default `'Report Subsystem'`.
[[ref/object/spf-report-settings#fromaddress|`fromAddress`]]: expression returning the email address used in the `From` header. Default `'noreply-spf@' + system('domain')`.
[[ref/object/spf-report-settings#subject|`subject`]]: expression returning the report subject. Default `'SPF Authentication Failure Report'`.
[[ref/object/spf-report-settings#sendfrequency|`sendFrequency`]]: expression returning the rate at which reports are sent to a given address. Default `[1, 1d]`. Returning `false` disables SPF failure reporting.
[[ref/object/spf-report-settings#dkimsigndomain|`dkimSignDomain`]]: expression returning the domain whose [[mta/authentication/dkim|DKIM]] signatures sign the outgoing report. Default `system('domain')`.

Example configuration using explicit values:

```
{  "fromName": {"else": "'Report Subsystem'"},  "fromAddress": {"else": "'[email protected]'"},  "subject": {"else": "'SPF Authentication Failure Report'"},  "sendFrequency": {"else": "[1, 1d]"},  "dkimSignDomain": {"else": "'example.org'"}}
```