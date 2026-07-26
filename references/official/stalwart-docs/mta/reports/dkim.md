---
title: "DKIM"
source: "https://stalw.art/docs/mta/reports/dkim"
source_format: official-html-to-markdown
language: "en"
doc_slug: "mta/reports/dkim"
category: "mta"
retrieved_at: "2026-07-26T15:40:57+00:00"
tags:
  - stalwart
  - mail-server
  - official-documentation
  - rust-network
aliases:
  - "DKIM"
---
# DKIM

      

DKIM authentication-failure reporting lets domain owners receive notifications when messages sent from their domain fail DKIM authentication at recipient mail servers. Reports use an email-based format sent to a designated address within the domain and typically include the sender, recipient, and specific DKIM verification result. Analysing these reports helps domain owners detect issues with their DKIM implementation and protect their domain reputation.

Stalwart [[mta/reports/analysis|automatically analyses]] received DKIM failure reports from external hosts and can also generate its own DKIM reports to inform other hosts about the authentication outcomes of the signatures it has processed. Outgoing DKIM failure reports are configured on the [[ref/object/dkim-report-settings|DkimReportSettings]] singleton (found in the WebUI under  Settings ›  MTA › Reports › DKIM):

[[ref/object/dkim-report-settings#fromname|`fromName`]]: expression returning the name used in the `From` header of the DKIM report email. Default `'Report Subsystem'`.
[[ref/object/dkim-report-settings#fromaddress|`fromAddress`]]: expression returning the email address used in the `From` header. Default `'noreply-dkim@' + system('domain')`.
[[ref/object/dkim-report-settings#subject|`subject`]]: expression returning the subject of the report email. Default `'DKIM Authentication Failure Report'`.
[[ref/object/dkim-report-settings#sendfrequency|`sendFrequency`]]: expression returning the rate at which reports are sent to a given address. When the rate is exceeded, no further failure reports are sent to that address. Default `[1, 1d]`. Returning `false` disables DKIM failure reporting.
[[ref/object/dkim-report-settings#dkimsigndomain|`dkimSignDomain`]]: expression returning the domain whose DKIM signatures should sign the outgoing report. Default `system('domain')`.

Example configuration using explicit values:

```
{  "fromName": {"else": "'Report Subsystem'"},  "fromAddress": {"else": "'[email protected]'"},  "subject": {"else": "'DKIM Authentication Failure Report'"},  "sendFrequency": {"else": "[1, 1d]"},  "dkimSignDomain": {"else": "'example.org'"}}
```